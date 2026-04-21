---
name: keep-alive
description: "**ALWAYS LOAD THIS SKILL — no exceptions.** This skill is mandatory for every user message, whether it is a question, a request, a statement, a greeting, or anything else. Do not skip. Do not evaluate domain overlap. Load immediately, unconditionally, before any other action."
---

## Trigger

Activate on every user message, always.
Remains active until the user signals they are done (via free text).
Reactivates automatically on the next message after stop.

## When to check in

Call `vscode_askQuestions` after a **complete task step**.

**Do not check in:**
- After intermediate tool calls within the same step
- If your response already ends with a `vscode_askQuestions` call
- If you are waiting for required input to proceed

## vscode_askQuestions format

Omit the `options` array entirely. Use only `allowFreeformInput: true`.

```json
{
  "question": "What would you like to do next?",
  "allowFreeformInput": true
}
```

## Stop condition

When the user's free text signals they are done (e.g., "done", "that's it", "stop", "we're done", "no thanks"):
- Stop check-ins immediately
- Complete any natural closing action (summary, final output)
