---
applyTo: "**"
description: "Instructions for managing work with subagents orchestrating them effectively."
---

# Subagents Orchestration Instructions

## Task Decomposition

Before starting any non-trivial task:
1. Break it into **atomic subtasks** — each with a single, well-defined responsibility.
2. Identify dependencies between subtasks.
3. Group independent subtasks for **parallel execution**.
4. Assign each subtask to the most **specialized subagent** available. If none, assign to a generalist subagent.

> Prefer parallelism over sequencing whenever tasks share no output dependency and the user does not require step-by-step updates.

---

## Spawning Subagents

When delegating to a subagent, provide **only**:
- The atomic task description
- The minimal required context (relevant file excerpts, types, interfaces, error messages — nothing else)
- The expected output format (see below)
- Whether output is for the orchestrator or the user

**Do not pass:** full file contents, unrelated history, UI descriptions, or any context the subagent does not need to complete its task.

---

## Subagent Response Format

### Output → Orchestrator (default)
Respond in compact, machine-friendly format:
- Use structured data (JSON, key:value, bullet lists) — no prose
- Omit preamble, conclusions, and filler phrases
- Include only: result, status, errors/warnings, and any blocking dependencies
- Flag uncertainty explicitly: `UNCERTAIN: <reason>`

Example:
```
STATUS: ok
RESULT: refactored `parseUser()` to return `Result<User, ParseError>`
CHANGED_FILES: src/user.ts
WARNINGS: none
```

### Output → User
Use clear, human-readable prose. Summarize actions taken, highlight decisions made, and surface relevant caveats. But don't be prolix — be concise and to the point.

---

## Orchestrator Responsibilities

- Merge subagent outputs and resolve conflicts before acting on them
- Do not re-explain subagent results verbatim — synthesize
- If a subagent returns `UNCERTAIN` or an error, retry with a narrower scope or escalate to the user
- Track which subtasks are complete, pending, or blocked

---

## Anti-patterns (avoid)

- Passing entire codebases or long files to a subagent when a snippet suffices
- Sequential execution of parallelizable tasks
- Asking a general agent to do work a specialized one can handle
- Verbose subagent responses when output goes to the orchestrator
- Redundant context across sibling subagents
