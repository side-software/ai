---
name: verify-contract
description: |
  The Verify Contract pattern: a single command that decides whether a task is
  done. Use this skill when setting up a new project's verification pipeline,
  when you need to understand what "done" means, or when deciding what checks
  belong in the verify command vs. elsewhere.
---

# The Verify Contract

## Why this skill exists

AI agents are particularly bad at deciding when they are finished. They declare victory on partial evidence, miss cascade effects, and conflate "it compiles" with "it works." Replacing the judgment call with a command removes this failure mode entirely.

The verify contract is: **a task is done when one command says it is done.**

## What belongs in the verify command

1. **Type check** — catches type errors and ripple effects
2. **Lint** — enforces style rules
3. **Unit tests** — regression safety
4. **Build** — proves the code ships

Must be fast enough that nobody skips it.

## What does NOT belong

- Anything requiring live infrastructure (Docker, databases, deploys)
- Anything slow enough developers skip it
- E2E tests needing a browser
- Manual inspection

## The agent recovery loop

1. Make changes
2. Run the verify command
3. Read failure output
4. Fix the specific failure
5. Run verify again
6. Repeat until green

## Common mistakes

- **Too slow**: Move slow checks to CI
- **Doesn't catch what matters**: If PRs keep breaking from X, add X to verify
- **Multiple commands**: One command. If different parts need different checks, the single command runs all of them
- **Requires infrastructure**: Keep verify infrastructure-free
