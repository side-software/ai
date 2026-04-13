# The AI Operating Model

This document explains the operating model that Side Software Co uses to deliver production-grade software with AI tools. The model is tool-agnostic — it works with Claude Code, Cursor, Antigravity, or any AI coding assistant that reads context from files.

## The core idea

Agents (human or AI) produce reliable work when they inherit the right constraints, reason against the right context, and can verify their own output before handing it off. The operating model encodes these three properties into reusable artifacts that travel with the codebase.

## Three pillars

### 1. Constraint over instruction

Encode non-negotiable rules in reusable documents (skills), not per-task prompts.

A skill is a markdown file that explains one architectural rule or procedure with enough context that an agent can apply it correctly. The key difference from documentation is that a skill explains the *why* — what breaks if you violate the rule, what the failure mode looks like. An agent that understands the failure mode will respect the constraint even in edge cases a bullet list wouldn't cover.

Skills live in the repo (`.claude/skills/` for Claude Code, `.agents/skills/` for Antigravity) and get read by the agent at the start of relevant tasks. They are cheap to maintain because they change only when the architecture changes.

**What this replaces**: long CLAUDE.md files full of rules nobody reads, per-task instructions that repeat the same constraints, and "style guides" that agents ignore because they don't explain consequences.

### 2. Verification over trust

A single command proves a task is done. No judgment calls.

The verify contract is the most leveraged rule in the model. AI agents are bad at deciding when they are finished — they declare victory on partial evidence, miss cascade effects, and conflate "it compiles" with "it works." A command that exits 0 or non-zero removes this failure mode entirely.

The verify command runs the checks the team considers mandatory: type check, lint, unit tests, build. It must be fast enough that nobody skips it (under 2-3 minutes). Anything slower belongs in CI.

**What this replaces**: "looks good to me" handoffs, agents that commit broken code, manual checklists that get skipped.

### 3. Guardrails over vigilance

Automated hooks catch mistakes before they ship.

Hooks fire before or after every tool call the agent makes:

- **PostToolUse hooks** (feedback): run a type check after every file edit. If the edit introduced an error, the agent sees it immediately — not 30 minutes later when verify runs.
- **PreToolUse hooks** (blocking): refuse tool calls that violate a rule. "Don't edit shadcn/ui components directly" isn't something the agent needs to remember — the hook enforces it.

Hooks are the per-edit counterpart to the per-task verify contract. Together they create a tight feedback loop: hooks catch errors in real time, verify confirms the final result.

**What this replaces**: watching the agent work, catching mistakes in code review, "you shouldn't have done that" feedback after the fact.

## How the pieces fit together

```
Skill (read once)            → Agent understands the rules
Hook (fires per edit)        → Agent gets instant feedback on mistakes
Verify (runs per task)       → Agent proves the task is done
Slash command / workflow      → Agent invokes procedures by name
Orchestration prompt          → Agent manages multi-phase work
```

A new project adopts the model by:
1. Writing a `project-conventions` skill with its architectural rules
2. Setting up a verify command
3. Adding hooks to `.claude/settings.json` (or equivalent)
4. Creating slash commands for recurring procedures

See `getting-started.md` for the step-by-step walkthrough.

## Agent orchestration

For work that spans multiple tasks, the model supports a phased orchestration pattern:

- **Independent chunks** (touch disjoint files) run in parallel — each on its own git branch, merged with `--no-ff` to preserve topology.
- **Coupled chunks** (share files or context) run sequentially — one agent keeps context warm.

The orchestrator decides per-phase whether to fan out or stay serial. The verify command runs after every merge. If it fails and the fix isn't obvious, the loop stops and hands control back to the human.

See `antigravity/workflows/orchestrator.md` or `claude/skills/verify-contract/SKILL.md` for the implementation templates.

## Cost awareness

Parallel agent teams are more expensive per phase (each agent has its own context window) but faster in wall-clock time. The model doesn't have built-in cost tracking yet — this is an acknowledged gap. The heuristic is: use parallel when chunks are truly independent and wall-clock savings matter; use sequential when context sharing would save tokens.
