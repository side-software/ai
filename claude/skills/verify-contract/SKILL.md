---
name: verify-contract
description: |
  The Verify Contract pattern: a single command that decides whether a task is
  done. Use this skill when setting up a new project's verification pipeline,
  when an agent needs to understand what "done" means, or when deciding what
  checks belong in the verify command vs. elsewhere.
---

# The Verify Contract

## Why this skill exists

AI agents are particularly bad at deciding when they are finished. They declare victory on partial evidence, miss cascade effects from their changes, and conflate "it compiles" with "it works." Replacing the judgment call with a command removes this failure mode entirely.

The verify contract is: **a task is done when one command says it is done.**

## What belongs in the verify command

The verify command runs the complete set of checks the team considers a hard requirement for the main branch being green:

1. **Type check** — catches type errors from the change and its ripple effects
2. **Lint** — enforces style rules that are too tedious to remember
3. **Unit tests** — regression safety for existing behavior
4. **Build** — proves the code actually ships

These checks must be fast enough that nobody skips them. If a check takes more than a few minutes, it belongs in CI, not in verify.

## What does NOT belong in the verify command

- Anything requiring live infrastructure (docker compose up, database migrations against a real server, deploy)
- Anything slow enough that developers would skip it
- End-to-end tests that need a browser or network
- Manual inspection ("looks right to me")

These are valid checks but they belong in a separate smoke test step, not in the verify gate.

## Canonical examples

**Node.js / pnpm monorepo:**
```bash
# package.json
"scripts": {
  "verify": "pnpm build && pnpm test"
}
```

**Astro project:**
```bash
npx astro check && npx astro build && npm test
```

**Python / FastAPI:**
```bash
ruff check . && mypy . && pytest
```

**Go:**
```bash
go vet ./... && go test ./...
```

## The agent recovery loop

When an agent uses the verify contract, the loop is:

1. Make changes
2. Run the verify command
3. Read the failure output
4. Fix the specific failure
5. Run verify again
6. Repeat until green

This loop is deterministic and self-correcting. The agent does not need judgment to know whether it is done — the command tells it.

## Setting up verify in a new project

1. Pick the checks that matter (type check, lint, test, build).
2. Wire them into a single command (`pnpm verify`, `make check`, a script).
3. Document the command in CLAUDE.md, .cursorrules, and any skill that references "done."
4. Add it to CI so the same command gates pull requests.
5. Add it to the `/verify` slash command (Claude Code) or `/verify` workflow (Antigravity).

## Common mistakes

- **Verify is too slow.** If it takes more than 2-3 minutes, developers and agents will skip it. Move the slow parts to CI.
- **Verify doesn't catch the thing that matters.** If PRs keep breaking because of X, and verify doesn't check X, add X to verify.
- **Multiple verify commands.** There should be exactly one. If different parts of the project have different checks, the verify command runs all of them.
- **Verify requires infrastructure.** If you need Docker running to verify, the command will fail in CI and in fresh clones. Keep verify infrastructure-free.
