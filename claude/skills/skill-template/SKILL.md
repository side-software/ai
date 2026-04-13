---
name: skill-template
description: |
  Template and guidance for writing new skills. Use this when creating a skill
  for a new project or a new task type. Covers the frontmatter format, section
  structure, and what makes a skill actually useful vs. just documentation.
---

# Writing a New Skill

## What makes a good skill

A skill is a self-contained markdown document encoding one concrete architectural piece — a rule, procedure, or troubleshooting flow — with enough context that an AI agent can apply it correctly without reading the entire codebase.

Good skills share four properties:

1. **They explain the "why" behind each rule.** "Don't import X in Y" is a rule. "Don't import X in Y because it breaks Z in this specific way" is a skill. The why is what lets an agent reason about edge cases.

2. **They name the failure mode.** What goes wrong if you violate this rule? What does the error look like? An agent that knows the failure mode will recognize it when it happens and self-correct.

3. **They include a "when rules seem to conflict" section.** Real tasks create tension between rules. The skill should say how to resolve the tension, not leave the agent guessing.

4. **They stay short enough to be read every time.** The two-minute rule: if reading the skill costs less than the rework from not reading it, it gets read. Skills longer than ~200 lines start getting skimmed.

## Skill types

**Gateway skill** — read first for any non-trivial change. Usually called `project-conventions`. Every project should have exactly one. It covers the architectural rules that apply everywhere.

**Per-task skill** — applies to a specific recurring task. Examples: `add-component`, `add-hook`, `extend-schema`, `load-data`, `troubleshoot-api`. These are narrower and more procedural.

## Template

```markdown
---
name: {{kebab-case-name}}
description: |
  {{One paragraph. State what the skill covers, what triggers it, and what
  rules it enforces. This description is what the agent sees when deciding
  whether to read the full skill — make it specific.}}
---

# {{Title}}

## Why this skill exists

{{What problem does this skill solve? What went wrong before it existed?
One or two paragraphs.}}

## The non-negotiables

{{For gateway skills: the rules, each with a "why" paragraph.}}
{{For per-task skills: the constraints specific to this task.}}

## Steps

{{For per-task skills: the procedure. Numbered steps, each with enough
context that an agent can execute without asking.}}

## What to read before writing

{{List the files the agent should read to understand the existing patterns
before making changes. Be specific — file paths, not "the relevant files."}}

## Common mistakes

{{What goes wrong most often? What does the agent typically get wrong on
the first try? List 3-5 concrete failure modes.}}

## When the rules seem to conflict with the task

{{How to resolve tension between this skill's rules and the task at hand.
The default: the rules are right, reshape the task.}}
```

## Where skills live

| Tool | Location |
|---|---|
| Claude Code | `.claude/skills/<skill-name>/SKILL.md` |
| Antigravity | `.agents/skills/<skill-name>/SKILL.md` |

Both tools use the same frontmatter format (`name`, `description`). A skill written for one can be used in the other with no changes.

## Skill vs. slash command vs. CLAUDE.md

- **Skill** = "why and how of a rule" — read by the agent when relevant
- **Slash command** = "invoke this procedure" — explicitly triggered
- **CLAUDE.md** = "always-on project context" — loaded every conversation

If something belongs in all three, put the detail in the skill and reference it from the other two. CLAUDE.md should be a routing table, not a novel.
