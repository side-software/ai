# CLAUDE.md

This repo is a **documentation project**, not a software project. There is no build step, no tests, no package manager. It contains markdown templates, JSON settings files, YAML workflows, and prose documentation for the AI operating model used across Side Software Co projects.

## What this repo is

The upstream reference for AI tool configurations. Individual project repos (storybook-components, stac-higher, etc.) copy and adapt templates from here. This repo is the single source of truth for portable patterns.

## Repo structure

```
claude/              Claude Code configs (skills, commands, settings, CLAUDE.md template)
cursor/              Cursor IDE configs (.cursorrules, .cursor/rules/*.mdc templates)
antigravity/         Antigravity configs (.agents/rules, .agents/skills, .agents/workflows)
github/              GitHub Actions (claude-agent.yml workflow, labels)
docs/                Prose docs (operating model, getting started, skill authoring)
```

## What kind of work happens here

1. **Updating templates** when the operating model evolves — a new hook pattern, a better skill structure, a refined workflow.
2. **Keeping configs current** with the latest documentation for each AI tool (Claude Code, Cursor, Antigravity). Tool APIs and config formats change; this repo must stay accurate.
3. **Extracting patterns** from project repos — when a project-specific skill proves reusable, the portable version gets added here.
4. **Writing and improving docs** — the `docs/` directory explains the operating model for adopters.

## Conventions

### File formats and their tools

| Extension | Format | Destination in consuming repos |
|---|---|---|
| `SKILL.md` | Markdown with YAML frontmatter (`name`, `description`) | `.claude/skills/` or `.agents/skills/` |
| `.md` in `claude/commands/` | Plain markdown | `.claude/commands/` |
| `.json` in `claude/settings/` | JSON (Claude Code settings format) | `.claude/settings.json` |
| `.mdc` in `cursor/rules/` | Markdown with YAML frontmatter (`description`, `globs`, `alwaysApply`) | `.cursor/rules/` |
| `.template` | Any format — copy and rename, removing `.template` suffix | Varies |
| `.md` in `antigravity/rules/` | Markdown with YAML frontmatter (`description`, `activation`) | `.agents/rules/` |
| `.md` in `antigravity/workflows/` | Markdown with title, description, numbered steps | `.agents/workflows/` |
| `.yml` in `github/workflows/` | GitHub Actions YAML | `.github/workflows/` |

### Placeholders

Templates use `{{PLACEHOLDER}}` markers (double curly braces, SCREAMING_SNAKE_CASE) for values that consuming repos fill in. Common placeholders:

- `{{PROJECT_NAME}}` — the project's name
- `{{VERIFY_COMMAND}}` — the project's verify command (e.g., `pnpm verify`)
- `{{DEV_COMMAND}}`, `{{BUILD_COMMAND}}`, `{{TEST_COMMAND}}` — development commands
- `{{RULE_NAME}}` — an architectural rule
- `{{STACK_DESCRIPTION}}` — tech stack summary

When adding new placeholders, use this format consistently. Don't use `<angle brackets>` or `[square brackets]` — those conflict with markdown/HTML.

### Skill format

Skills must have YAML frontmatter with at least `description`. The `name` field defaults to the folder name if omitted. The description is what the agent sees when deciding whether to read the full skill — make it specific.

```yaml
---
name: kebab-case-name
description: |
  What this skill does and when to use it.
---
```

Both Claude Code and Antigravity use this same format. Write once, deploy to either `.claude/skills/` or `.agents/skills/`.

### Cross-tool parity

When a concept exists in multiple tools, maintain parallel templates:

| Concept | Claude Code | Cursor | Antigravity |
|---|---|---|---|
| Project rules | `claude/skills/project-conventions/` | `cursor/rules/conventions.mdc` | `antigravity/rules/conventions.md` |
| Project overview | `claude/CLAUDE.md.template` | `cursor/rules/project-overview.mdc` | `antigravity/rules/project-overview.md` |
| Verify procedure | `claude/commands/verify.md` | — | `antigravity/workflows/verify.md` |
| New component | `claude/commands/new-component.md` | — | `antigravity/workflows/new-component.md` |

When updating one, check if the parallel versions need the same change.

## Editing guidelines

- **Don't add project-specific rules.** Everything here must be portable. If it mentions a specific repo (storybook-components, stac-higher), it's an example in a comment, not a rule.
- **Keep skills under 200 lines.** The two-minute rule: if reading it costs less than the rework from not reading it, it gets read.
- **Preserve the `{{PLACEHOLDER}}` format.** Consuming repos rely on being able to grep for `{{` to find what to customize.
- **JSON must be valid.** After editing any `.json` file, verify with `python3 -c "import json; json.load(open('path/to/file.json'))"`.
- **YAML must be valid.** The GitHub Actions workflow in particular is sensitive to indentation.
- **No hardcoded paths.** Grep for `/Users/` before committing. Use `$CLAUDE_PROJECT_DIR` for Claude Code paths.

## Keeping up with tool documentation

Each AI tool's config format evolves. When updating templates to match current docs:

- **Claude Code**: https://docs.anthropic.com/en/docs/claude-code — skills, hooks (PostToolUse/PreToolUse), settings.json format, slash commands, MCP
- **Cursor**: https://docs.cursor.com — .cursorrules format, .cursor/rules/*.mdc frontmatter fields (description, globs, alwaysApply), @-mentions
- **Antigravity**: https://antigravity.google/docs — .agents/rules/ (activation modes: Manual, Always On, Model Decision, Glob), .agents/skills/ (SKILL.md format), .agents/workflows/ (slash-invokable steps), 12,000 char limit per file

When a tool changes its config format, update the corresponding templates AND update this section with the correct documentation URL.

## Where patterns come from

Active project repos where the operating model is used in production:

- **[ogc-maps/storybook-components](https://github.com/ogc-maps/storybook-components)** — the most mature config set. 6 skills, PostToolUse/PreToolUse hooks, agent teams, Ralph Loop orchestration, 5-mode GitHub Actions workflow.
- **[ogc-maps/stac-higher](https://github.com/ogc-maps/stac-higher)** — Astro + React. 5 slash commands, astro check hooks, TODO.md agent loop protocol.
- **[ogc-maps/gunnison-county-map](https://github.com/ogc-maps/gunnison-county-map)** — simpler deployment. 5 Cursor rules, basic CLAUDE.md.

When extracting a pattern from a project repo: strip project-specific details, replace concrete values with `{{PLACEHOLDER}}` markers, and add the portable version here. Don't modify the source repo — each project owns its configs.
