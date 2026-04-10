# Plan Blueprint — Reference

## Risk rubric

| Level | Emoji | Criteria |
|-------|-------|----------|
| LOW | 🟢 | Fully reversible. Isolated to one concern. Well-understood change with no unknowns. Failure has no user-facing impact. |
| MEDIUM | 🟡 | Moderately reversible. Touches shared code or config. Some uncertainty in scope or behaviour. Failure may affect a subset of users or flows. |
| HIGH | 🟠 | Hard or slow to reverse. Cross-cutting change (e.g. schema migration, public API change, auth logic). Significant unknowns. Failure has broad user-facing impact. |
| CRITICAL | 🔴 | Irreversible or production-impacting without an incident. Touches security, billing, compliance, or core data integrity. Any mistake may require a hotfix or rollback under pressure. |

---

## Effort scale (Fibonacci story points)

| SP | Meaning |
|----|---------|
| 1 | Trivial — a config change, a rename, a one-liner. Under 30 minutes. |
| 2 | Small — a simple function or a single well-understood file change. Under 1 hour. |
| 3 | Moderate — a few files, clear requirements, no significant unknowns. Half a day. |
| 5 | Large — multiple files or layers, some design decisions needed. Full day. |
| 8 | Very large — cross-cutting or involves coordination, meaningful uncertainty. 2–3 days. |
| 13 | Uncertain — scope is not fully understood; consider breaking down further before starting. |

---

## ASCII dependency diagram rules

- Direction: top-to-bottom only.
- No arrows (`▼`) anywhere — pure lines only.
- Lines feeding into a merge connector stay as plain `│`.
- Convergence point: `└───┬───┘`
- Parallel fork (two branches): `┌──────┴──────┐`
- Parallel fork (three branches): `┌────────┼────────┐`
- Steps are written as plain text nodes, not boxes.
- Always open with `[Start]` and close with `[Done]`.

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
