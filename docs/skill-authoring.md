# Skill Authoring

How to write skills that actually make AI agents produce better output.

## What a skill is

A skill is a self-contained markdown document encoding one concrete architectural piece — a rule, procedure, or troubleshooting flow — with enough context that an AI agent can apply it correctly without reading the entire codebase.

Skills are not documentation. Documentation explains how things work. Skills encode constraints that prevent things from breaking, with the reasoning that lets an agent navigate edge cases.

## The four properties of a good skill

### 1. It explains the "why" behind each rule

Bad: "Don't import MapLibre in the library."
Good: "Don't import MapLibre in the library because the admin app would pull it into its bundle for no reason, vitest blows up in jsdom, and external consumers are forced into our renderer choice."

The "why" is what lets an agent reason about edge cases. A bare rule gets violated the moment the task seems to require it. A rule with a failure mode gets respected because the agent understands the consequences.

### 2. It names the failure mode

What goes wrong if you violate this rule? What does the error look like? An agent that knows the failure mode will recognize it when it happens and self-correct.

"If you skip Zod validation, the field silently disappears when the admin app round-trips the config on save" is more useful than "always validate with Zod."

### 3. It includes a "when rules conflict" section

Real tasks create tension between rules. The skill should say how to resolve the tension, not leave the agent guessing. The default: the rules are right, reshape the task.

### 4. It stays short enough to be read every time

The two-minute rule: if reading the skill costs less than the rework from not reading it, the skill gets read. Skills longer than 200 lines start getting skimmed. If yours is longer, split it into a gateway skill and per-task skills.

## Two types of skills

**Gateway skill** — read first for any non-trivial change. Usually called `project-conventions`. Every project should have exactly one. It covers the rules that apply everywhere.

**Per-task skill** — applies to a specific recurring task. Examples: `add-component`, `add-hook`, `extend-schema`, `load-data`, `troubleshoot-api`. These are narrower and more procedural.

## Template structure

```markdown
---
name: kebab-case-name
description: |
  One paragraph. What the skill covers, what triggers it, what
  rules it enforces. This is what the agent sees when deciding
  whether to read the full skill.
---

# Title

## Why this skill exists
What problem does it solve? What went wrong before it existed?

## The non-negotiables
For gateway skills: the rules with "why" paragraphs.
For per-task skills: the constraints specific to this task.

## Steps
For per-task skills: the procedure. Numbered, self-contained.

## What to read before writing
Specific file paths the agent should read first.

## Common mistakes
3-5 concrete failure modes that happen most often.

## When the rules seem to conflict
How to resolve tension between this skill and the task.
```

## Where skills live

| Tool | Location |
|---|---|
| Claude Code | `.claude/skills/<skill-name>/SKILL.md` |
| Antigravity | `.agents/skills/<skill-name>/SKILL.md` |

Both use the same frontmatter format. A skill written for one works in the other.

## Skills vs. other artifacts

| Artifact | Purpose | When it's read |
|---|---|---|
| Skill | Why and how of a rule | When relevant to the task |
| Slash command / workflow | Invoke a procedure | When explicitly triggered |
| CLAUDE.md | Always-on project context | Every conversation |
| Hook | Automated check | Every tool call |

If something belongs in multiple places, put the detail in the skill and reference it from the others. CLAUDE.md should be a routing table, not a novel.

## Evolving skills

Skills improve over time. After a skill fails to prevent a mistake:

1. Identify what the agent got wrong.
2. Add the failure mode to the "common mistakes" section.
3. If the rule was unclear, rewrite the "why" paragraph.
4. If the rule was missing, add it.

The goal is that each skill gets better at preventing the specific mistakes that happen in practice, not that it covers every hypothetical.
