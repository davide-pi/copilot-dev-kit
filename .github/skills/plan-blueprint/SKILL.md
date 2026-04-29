---
name: plan-blueprint
description: Produce a structured, atomic implementation plan with risk levels, dependency diagrams, and a summary table. Use when asked to plan, create a plan, blueprint a feature, or break down a task into steps.
---

# Plan Blueprint

## Plan Header

```
## Plan: {Title}

**Goal** — One sentence.
**Total Effort** — N SP
**Risk Summary** — 🟢 N · 🟡 N · 🟠 N · 🔴 N

**Assumptions**
- ...
```

## Step Template

```
### **Step {N} – [{risk emoji + level}] – {title}**

**What** — One or two sentences. Name exact files, functions, data structures, or config keys. Enough detail for the implementer to start without re-reading the full plan.

**Why** — Business or technical reason. Explicit dependency references (e.g. "prerequisite for Step 3").

**Files** — `path/to/file.ext` · `path/to/other.ext`

**Risk** — {🟢 LOW | 🟡 MEDIUM | 🟠 HIGH | 🔴 CRITICAL} · {reason} · **Mitigation**: {action}

**Effort** — N SP

**Verify**
⚙️ `{exact shell command}` — or "N/A — toolchain unknown"
👁️ {Manual check}
```

## Workflow

1. **Decompose** — Atomic steps: minimal, self-contained, indivisible, producing a verifiable result.
2. **Sequence** — Deliver value earliest, minimise rework. Identify parallelisable steps.
3. **Assess risk** — Per the rubric in [REFERENCE.md](REFERENCE.md). Include reason + mitigation.
4. **Assign effort** — Fibonacci SP per scale in [REFERENCE.md](REFERENCE.md).
5. **Write steps** — Use the template above. No section may be omitted.
6. **Draw diagram** — Top-to-bottom ASCII dependency diagram per [REFERENCE.md](REFERENCE.md) rules.
7. **Summary table**:

```
| Step | Risk | Effort | Key File | Change |
|------|------|--------|----------|--------|
| 1    | 🟢 LOW | 2 SP | `path/file` | Short description |
```

## Reference

- Risk rubric, ASCII diagram patterns, effort scale → [REFERENCE.md](REFERENCE.md)
- Worked example → [EXAMPLES.md](EXAMPLES.md)
