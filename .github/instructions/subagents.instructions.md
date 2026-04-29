---
applyTo: "**"
description: "Instructions for managing work with subagents orchestrating them effectively."
---

# Subagents Orchestration

## Task Decomposition

Before starting any non-trivial task:
1. Break into **atomic subtasks** — single responsibility each.
2. Identify dependencies between subtasks.
3. Group independent subtasks for **parallel execution**.
4. Assign to the most **specialized subagent** available; fall back to generalist.

> Prefer parallelism over sequencing when tasks share no output dependency.

## Spawning Subagents

Provide **only**:
- Atomic task description
- Minimal required context (relevant excerpts, types, error messages)
- Expected output format
- Whether output targets the orchestrator or the user

**Never pass:** full file contents, unrelated history, UI descriptions, or unnecessary context.

## Subagent Response Format

### → Orchestrator (default)
Compact, machine-friendly:
- Structured data (JSON, key:value, bullet lists) — no prose
- Include only: result, status, errors/warnings, blocking dependencies
- Flag uncertainty: `UNCERTAIN: <reason>`

```
STATUS: ok
RESULT: refactored `parseUser()` → `Result<User, ParseError>`
CHANGED_FILES: src/user.ts
WARNINGS: none
```

### → User
Concise human-readable prose. Summarize actions, highlight decisions, surface caveats.

## Orchestrator Responsibilities

- Merge and synthesize subagent outputs — do not echo verbatim
- On `UNCERTAIN` or error: retry with narrower scope or escalate to user
- Track subtask status: complete, pending, blocked

## Anti-patterns

- Passing entire files when a snippet suffices
- Sequential execution of parallelizable tasks
- General agent doing specialized work
- Verbose subagent responses to orchestrator
- Redundant context across sibling subagents
