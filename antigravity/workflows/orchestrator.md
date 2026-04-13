# Orchestrator

Execute a phased development plan with parallel and sequential work.

This workflow is for multi-task plans where some chunks can run in parallel (touch disjoint files) and others must run sequentially (share context or files).

## Setup

1. Read the plan document (e.g., `PLAN.md` or the issue description) to understand all phases.
2. Read the project conventions skill.
3. Confirm the main branch is clean and the verify command passes.

## Phase execution

For each phase in the plan:

### Parallel phases (independent chunks)

When a phase contains chunks that touch disjoint files:

1. Create a branch for each chunk from the main working branch.
2. Implement each chunk on its own branch.
3. Run the verify command on each branch.
4. Merge each branch back with `--no-ff` (preserves parallel-work topology in git history).
5. Run the verify command after all merges. Fix obvious merge damage or stop if ambiguous.

### Sequential phases (coupled chunks)

When a phase contains chunks that share files or context:

1. Create a single branch for the phase.
2. Work through chunks in order, committing after each.
3. Run the verify command after all chunks are complete.
4. Merge back with `--no-ff`.

## Between phases

1. Confirm the main branch is clean.
2. Run the verify command as a sanity check.
3. If the phase touched infrastructure (DB schema, Docker), run a smoke test.
4. Log phase completion.

## Stop conditions

Halt and report to the human if:

- A merge conflict is ambiguous (unsure which change to keep)
- The verify command fails and the fix is not obvious
- Requirements are ambiguous or a prerequisite is broken
- A chunk fails to complete

When stopping, report: which phase/chunk failed, the exact error, branch states, and a suggested next step.
