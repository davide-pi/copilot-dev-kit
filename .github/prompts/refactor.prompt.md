---
mode: agent
description: Step-by-step guided refactoring with explicit rationale for each change and guaranteed behavioural equivalence.
tools:
  - read_file
  - semantic_search
  - grep_search
---

# Refactor

## Step 1 – Read

`read_file` the target file(s) in full. If not specified, ask before proceeding.

## Step 2 – Identify opportunities

List each candidate with one line: readability issues (functions > 20 lines, unclear names, deep nesting, magic values), duplication, SOLID violations, excessive complexity.

## Step 3 – Plan atomic steps

Each step changes exactly one thing, preserves observable behaviour, and leaves the codebase compilable. Use:

```
**Step N – <Refactoring type>: <title>**
What: <one sentence>
Rationale: <why this improves the code>
Risk: 🟢 [LOW] / 🟡 [MEDIUM]
Verify: <how to confirm behaviour is preserved>
```

## Step 4 – Implement

For each step: **Before** snippet → **After** snippet → **Why** (one sentence).

## Rules

- Never mix refactoring with behaviour changes. Note bugs separately.
- Never rename a public API without flagging it as a breaking change.
- Preserve all existing tests; explain any that must change.
- Confirm with the user before any 🟡 [MEDIUM] or higher step.
