# Getting Started

How to adopt the AI operating model in a new project.

## Prerequisites

- A git repository with a working build/test pipeline
- At least one AI coding tool installed (Claude Code, Cursor, or Antigravity)
- 30 minutes

## Step 1: Set up the verify command

The verify contract is the foundation. Everything else builds on it.

1. Decide what checks matter: type check, lint, unit tests, build.
2. Wire them into a single command. Examples:
   - `pnpm verify` (package.json script running `pnpm build && pnpm test`)
   - `npx astro check && npx astro build && npm test`
   - `make check` (Makefile combining ruff, mypy, pytest)
3. Run it. It should take under 2-3 minutes. If it's slower, move slow checks to CI.
4. Add it to CI so the same command gates pull requests.

**Template**: Copy `claude/commands/verify.md` into your project's `.claude/commands/verify.md` and fill in the commands.

## Step 2: Write the gateway skill

The `project-conventions` skill is the single document an agent reads before any non-trivial change. It contains the architectural rules with the reasoning behind each one.

1. Copy `claude/skills/project-conventions/SKILL.md` into your project's `.claude/skills/project-conventions/SKILL.md`.
2. Replace the `{{PLACEHOLDER}}` sections with your project's actual rules.
3. For each rule, write the "why" — what breaks if the rule is violated.
4. Keep it under 200 lines. If it's longer, split off per-task skills.

**Guidance**: Read `skill-authoring.md` for what makes a good skill.

## Step 3: Write CLAUDE.md

CLAUDE.md is the always-on context — loaded into every conversation. It should be a routing table, not a novel.

1. Copy `claude/CLAUDE.md.template` into your project root as `CLAUDE.md`.
2. Fill in: commands, architecture overview, key directories, conventions, skill references.
3. Keep it concise. Point to skills for detail.

## Step 4: Add hooks (Claude Code)

Hooks give the agent instant feedback on mistakes.

1. Copy `claude/settings/settings-base.json` as your project's `.claude/settings.json`.
2. Add the permissions your project needs (build commands, test commands, etc.).
3. If your project uses TypeScript/Astro, merge in hooks from `claude/settings/settings-astro.json`.
4. Customize the PostToolUse hook to run your project's type check command.
5. Add PreToolUse hooks for any files that shouldn't be edited directly.

## Step 5: Add Cursor rules (optional)

If you use Cursor:

1. Copy `cursor/cursorrules.template` as your project's `.cursorrules`.
2. Copy templates from `cursor/rules/` into `.cursor/rules/`.
3. Fill in the placeholders.
4. Point rules to the same skills that Claude Code uses — they share the `.claude/skills/` directory.

## Step 6: Add Antigravity configs (optional)

If you use Antigravity:

1. Copy templates from `antigravity/rules/` into your project's `.agents/rules/`.
2. Copy skills from `antigravity/skills/` into `.agents/skills/`. (The SKILL.md format is identical to Claude Code's — you can share the same files.)
3. Copy workflows from `antigravity/workflows/` into `.agents/workflows/` and customize.

## Step 7: Add the GitHub Actions workflow (optional)

If you want AI-assisted issue triage and PR review:

1. Copy `github/workflows/claude-agent.yml` into your project's `.github/workflows/`.
2. Uncomment and configure the package manager setup step.
3. Add `ANTHROPIC_API_KEY` as a repository secret.
4. Create the status labels (see `github/labels.yml` for the `gh label create` commands).

## Step 8: Add per-task skills

As you build, you'll notice recurring tasks (adding components, extending schemas, loading data, troubleshooting). For each:

1. Read `skill-authoring.md`.
2. Copy `claude/skills/skill-template/SKILL.md` as a starting point.
3. Write the skill based on what you've learned.
4. Add a slash command (Claude Code) or workflow (Antigravity) that invokes the skill.

## What to do when things drift

This repo is the upstream reference. Your project's configs will diverge — that's expected. Periodically compare your configs against the templates here. If a pattern has improved upstream, pull the improvement manually. The model is intentionally not automated at this layer — each project owns its configs.
