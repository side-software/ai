---
name: project-conventions
description: |
  The non-negotiable architectural rules for this project, with the reasoning
  behind each one. Read this skill before any non-trivial code change — before
  adding components, hooks, store fields, schema fields, or anything that
  crosses a module boundary. The rules look like preferences but each exists
  because violating it caused a real problem. Reading this before writing code
  is cheaper than discovering the rule during review.
---

# Project Conventions

## Why this skill exists

Any contributor — human or AI — should be able to land a change that respects the architecture without reading the entire codebase first. This skill is the long-form version of the rules, with the *why*, so that when a rule and a goal seem to conflict you can reason about which constraint is actually load-bearing.

If you are about to write code and have not read this skill yet, stop and read it. It takes two minutes and prevents most rework.

## The non-negotiables

<!-- ============================================================
     Replace these with your project's actual rules. Each rule
     should follow this structure:

     ### N. Rule name (short, imperative)

     > One-line summary of the constraint.

     Explanation of WHY the rule exists — what went wrong when it
     was violated, or what breaks if it's ignored. This is the part
     that makes the rule stick: an agent that understands the failure
     mode will respect the constraint even in edge cases.

     **Where to put it instead:** If the rule says "don't do X here",
     explain where X does belong.
     ============================================================ -->

### 1. {{RULE_NAME}}

> {{One-line constraint}}

{{Why this rule exists. What breaks if you violate it. What the failure mode looks like.}}

**Where to put it instead:** {{Redirect — where does the prohibited thing actually belong?}}

### 2. {{RULE_NAME}}

> {{One-line constraint}}

{{Why this rule exists.}}

### 3. Always run the verify command before declaring a task done

> `{{VERIFY_COMMAND}}` must exit 0 before you hand off.

This is the contract the automation pipeline relies on. Both the local workflow and the GitHub Action expect that "I'm finished" means the verify command passes. Don't hand off, open a PR, or say "ready for review" until you have run it.

If the verify command fails, fix the failure. If the failure is real and out of scope, say so explicitly rather than silently skipping.

## When the rules seem to conflict with the task

The rules above describe constraints, not goals. If you find yourself wanting to violate one, pause and re-read the *why* paragraph. Almost always there is a way to achieve the goal without violating the constraint.

If the rules genuinely block a task, that is a real conversation to have — but the default assumption should be that the rules are right and the task needs to be reshaped, not the other way around.

## Other skills that go with this one

Once you know the rules, per-task skills tell you how to apply them:

<!-- List your project's per-task skills here, e.g.:
- `add-component` — adding a UI component
- `add-hook` — adding a data-fetching hook
- `extend-schema` — changing a shared schema and propagating
- `load-data` — ingesting data into the database
-->
