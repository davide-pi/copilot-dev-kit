---
name: plan-blueprint
description: Produce a structured, atomic implementation plan with risk levels, dependency diagrams, and a summary table. Use when asked to plan, create a plan, blueprint a feature, or break down a task into steps.
---

# Plan Blueprint

## Plan header

Every plan opens with this block:

```
## Plan: {Title}

**Goal** — One sentence describing the desired outcome.
**Total Effort** — N SP
**Risk Summary** — 🟢 N · 🟡 N · 🟠 N · 🔴 N

**Assumptions**
- ...
```

## Step template

```
### **Step {N} – [{risk emoji + level}] – {title}**

**What** — One or two sentences. Name the exact files, functions, data structures, or config keys involved. Give the implementing agent enough detail to start without re-reading the whole plan.

**Why** — Business or technical reason. Call out dependencies on other steps explicitly (e.g. "prerequisite for Step 3").

**Files** — `path/to/file.ext` · `path/to/other.ext`

**Risk** — {🟢 LOW | 🟡 MEDIUM | 🟠 HIGH | 🔴 CRITICAL} · {reason} · **Mitigation**: {what to do}

**Effort** — N SP

**Verify**
⚙️ `{exact shell command}` — or "N/A — toolchain unknown"
👁️ {Open-ended manual check for the user}
```

## Workflow

Follow this sequence every time you produce a plan:

1. **Decompose** — Break the work into atomic steps. Each step must be a minimal, self-contained, indivisible unit of work that produces a verifiable result and requires no further subdivision to be performed correctly.
2. **Sequence** — Order steps to deliver value earliest and minimise rework risk. Identify which steps are independent (can run in parallel).
3. **Assess risk** — Assign a risk level to each step using the rubric in [REFERENCE.md](REFERENCE.md). Include reason and mitigation.
4. **Assign effort** — Estimate in Fibonacci story points using the scale in [REFERENCE.md](REFERENCE.md).
5. **Write steps** — Use the step template above. No section may be omitted.
6. **Draw diagram** — Produce a top-to-bottom ASCII dependency diagram after all steps. Follow the diagram rules in [REFERENCE.md](REFERENCE.md) exactly.
7. **Write summary table** — After the diagram:

```
| Step | Risk | Effort | Key File | Change |
|------|------|--------|----------|--------|
| 1    | 🟢 LOW | 2 SP | `path/file` | Short description |
```

## Reference

- Risk rubric, ASCII diagram patterns, effort scale → [REFERENCE.md](REFERENCE.md)
- Fully-worked example plan → [EXAMPLES.md](EXAMPLES.md)
