---
description: Architecture rules for {{PROJECT_NAME}} source code. Applied when editing source files.
activation: model_decision
---

# Conventions

These rules apply to all source code changes. Each exists because violating it caused a real problem. Read `.agents/skills/project-conventions/SKILL.md` for the full reasoning behind each one.

- {{Convention 1 — e.g., "Never import X in Y code"}}
- {{Convention 2 — e.g., "Keep domain state controlled via props and callbacks"}}
- {{Convention 3 — e.g., "Use the project's Tailwind prefix in library components"}}
- Prefer existing hooks and utilities over building new ones
- Export new APIs from barrel files and package entry points
- Always run `{{VERIFY_COMMAND}}` before declaring a task done

For task-specific guidance, use the matching skill:
- {{Task type}}: `.agents/skills/{{skill-name}}/SKILL.md`
