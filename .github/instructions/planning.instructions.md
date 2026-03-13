---
applyTo: "**"
---

# Planning

For any planning/design request, decompose into **atomic steps** and use the format below.

## Rules
- One step = one change (one class / method / file / migration). If a step touches multiple concerns, split it.
- Each step is independently implementable and compiles with passing tests before the next begins.
- Foundations first: interfaces, contracts, migrations before consumers.
- Refactors strictly separate from behaviour changes.
- Every step has a **Verify** clause (test to run, build check, or observable output).
- No speculative steps; no god steps; no mixed concerns.
- Choose the sequence that delivers value earliest and minimises rework risk.

## Risk levels

| Level | Meaning |
|-------|---------|
| 🟢 **[LOW]** | Minimal impact; easy to revert |
| 🟡 **[MEDIUM]** | Moderate impact; careful review needed |
| 🟠 **[HIGH]** | Significant impact; test thoroughly |
| 🔴 **[CRITICAL]** | Breaking or data-loss risk; do not proceed without validation |

## Full step format

```
**Step N – <title>**
What: <one sentence>
Files: <affected files>
Why: <why this step is needed>
Risk: {level} — <what could go wrong>
Verify: <test / build check / observable output>
```

After the full plan, always offer:
> Reply `implement` to start implementation

## Dependency schema

After the full plan, include an ASCII dependency graph showing which steps must complete before others. Root-level steps have `(no deps)`. Use `├─`/`└─` for children, `─┐`/`─┴─` for convergence.

## Changes summary

After the full plan, include a table ordered by implementation sequence:

| Step | Risk | File | Change |
|------|------|------|--------|
