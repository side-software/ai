# ai

Shared AI workflow configurations for [Side Software Co](https://side-software.github.io/) projects. This repo is the upstream reference for the AI operating model — portable patterns that get adopted into individual project repos.

## What this is

A library of configuration templates for three AI coding tools and one CI/CD integration:

| Directory | Tool | What it contains |
|---|---|---|
| `claude/` | [Claude Code](https://claude.ai/code) | Skills, slash commands, settings with hooks |
| `cursor/` | [Cursor](https://cursor.com) | `.cursorrules` and `.cursor/rules/*.mdc` templates |
| `antigravity/` | [Antigravity](https://antigravity.google) | `.agents/rules/`, `.agents/skills/`, `.agents/workflows/` |
| `github/` | GitHub Actions | Claude agent workflow (5 modes: clarify, plan, implement, review, default) |

Plus `docs/` explaining the operating model behind it all.

## What this is not

This repo doesn't replace per-project configs. Each project still has its own `.claude/`, `.cursor/`, and `.agents/` directories with project-specific rules. This repo is where the **portable patterns** live — the ones that get copied and adapted, not imported directly.

## How to use it

### Starting a new project

1. Read `docs/getting-started.md` for the full walkthrough.
2. Copy the templates you need into your project's config directories.
3. Fill in the project-specific placeholders (marked with `{{PLACEHOLDER}}`).
4. Customize the verify command, stack details, and conventions for your project.

### Updating an existing project

Compare your project's configs against the templates here. If a pattern has improved upstream, pull the improvement into your project manually. This is intentionally not automated — each project owns its configs.

## The operating model

The configs in this repo encode an [AI Operating Model](docs/operating-model.md) built on three ideas:

1. **Constraint over instruction** — encode rules in reusable documents (skills), not per-task prompts
2. **Verification over trust** — a single command proves a task is done (the verify contract)
3. **Guardrails over vigilance** — automated hooks catch mistakes before they ship

See `docs/operating-model.md` for the full explanation.

## Directory overview

```
claude/
  skills/                 Portable skill templates (.claude/skills/ format)
  commands/               Slash command templates (.claude/commands/ format)
  settings/               Settings JSON with hook patterns
  CLAUDE.md.template      Starter CLAUDE.md

cursor/
  cursorrules.template    Top-level .cursorrules routing file
  rules/                  .cursor/rules/*.mdc templates

antigravity/
  rules/                  .agents/rules/ templates (Always On, Model Decision, Glob)
  skills/                 .agents/skills/ templates (same SKILL.md format as Claude)
  workflows/              .agents/workflows/ templates (invoked via /workflow-name)

github/
  workflows/              GitHub Actions workflow (reference, not active here)
  labels.yml              Label definitions for the workflow

docs/
  operating-model.md      The three pillars explained
  getting-started.md      How to adopt this in a new repo
  skill-authoring.md      How to write good skills
```

## Related

- [ogc-maps](https://github.com/ogc-maps) — open-source geospatial libraries where this model is actively used
- [Side Software Co](https://side-software.github.io/) — the business behind this work
