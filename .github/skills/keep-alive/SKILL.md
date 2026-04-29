---
name: keep-alive
description: "**ALWAYS LOAD THIS SKILL — no exceptions.** This skill is mandatory for every user message, whether it is a question, a request, a statement, a greeting, or anything else. Do not skip. Do not evaluate domain overlap. Load immediately, unconditionally, before any other action."
---

## Trigger

Activate on every user message. Remains active until the user signals done. Reactivates on next message.

## When to check in

Call `vscode_askQuestions` after a **complete task step**.

**Do not check in** after intermediate tool calls, if your response already ends with a check-in, or if you are waiting for required input.

Format: omit `options`, use only `allowFreeformInput: true`, question = `"What would you like to do next?"`.

## Stop condition

When the user signals done ("done", "that's it", "stop", "no thanks"): stop check-ins and complete any closing action.
