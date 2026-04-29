# Plan Blueprint — Reference

## Risk Rubric

| Level | Emoji | Criteria |
|-------|-------|----------|
| LOW | 🟢 | Fully reversible. Isolated. Well-understood. No user-facing impact on failure. |
| MEDIUM | 🟡 | Moderately reversible. Touches shared code/config. Some scope uncertainty. May affect subset of users. |
| HIGH | 🟠 | Hard to reverse. Cross-cutting (schema, public API, auth). Significant unknowns. Broad user impact. |
| CRITICAL | 🔴 | Irreversible or production-impacting. Touches security, billing, compliance, or data integrity. Mistake requires hotfix/rollback. |

## Effort Scale (Fibonacci SP)

| SP | Meaning |
|----|---------|
| 1 | Trivial — config change, rename, one-liner. <30 min. |
| 2 | Small — single function, one well-understood file. <1 hour. |
| 3 | Moderate — few files, clear requirements, no unknowns. Half day. |
| 5 | Large — multiple files/layers, some design decisions. Full day. |
| 8 | Very large — cross-cutting, coordination needed, meaningful uncertainty. 2–3 days. |
| 13 | Uncertain — scope not fully understood. Consider breaking down further. |

## ASCII Dependency Diagram Rules

- Top-to-bottom only. No arrows (`▼`).
- Plain `│` lines into merge connectors.
- Convergence: `└───┬───┘`
- Two-way fork: `┌──────┴──────┐`
- Three-way fork: `┌────────┼────────┐`
- Steps as plain text, not boxes.
- Open with `[Start]`, close with `[Done]`.

### Pattern 1 — Sequential chain

```
     [Start]
        │
      Step 1
        │
      Step 2
        │
      Step 3
        │
      [Done]
```

### Pattern 2 — Parallel fork-join (asymmetric depth)

```
        [Start]
           │
    ┌──────┴──────┐
    │             │
  Step 1       Step 2
    │             │
  Step 3          │
    └──────┬──────┘
           │
         Step 4
           │
         [Done]
```

### Pattern 3 — Three-way fork, two converge early

```
              [Start]
                 │
        ┌────────┼────────┐
        │        │        │
     Step 1   Step 2   Step 3
        └───┬───┘         │
            │             │
         Step 4           │
            └──────┬──────┘
                   │
                Step 5
                   │
                [Done]
```
