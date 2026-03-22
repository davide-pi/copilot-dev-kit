# Agent Pipeline

Domain knowledge on the `team-leader` agent pipeline — flows, handoff contracts, cascade rules, and plan file schema.

---

## Request Types

| Type | Definition |
|---|---|
| `feature` | New capability or any modification/improvement to existing behaviour |
| `bug` | A defect: something broken that must be fixed without changing working behaviour |

---

## Flows

### Feature Flow
New functionality or modifications to existing code. Architect always explores codebase first. `plan-writer(section)` is invoked after **each** agent to persist its output immediately.
```
kickoff-manager → plan-writer(section) → architect-analyst → plan-writer(section)
→ senior-dev-analyst → plan-writer(section) → quality-assurance-analyst → plan-writer(section)
→ plan-writer(finalize)
→ [USER APPROVES] → plan-writer(status-update: Approved)
→ senior-dev (task-by-task) → [optional] quality-assurance-dev
```

### Bug Flow
Defects only. Architect invoked only when structural change is needed.
```
kickoff-manager → plan-writer(section) → bug-investigator → plan-writer(section: Decisions)
→ architect-analyst (only if structural impact) → plan-writer(section)
→ senior-dev-analyst → plan-writer(section) → quality-assurance-analyst → plan-writer(section)
→ plan-writer(finalize)
→ [USER APPROVES] → plan-writer(status-update: Approved)
→ senior-dev (task-by-task) → [optional] quality-assurance-dev
```

---

## Complexity Depth

| Level | Signals | Decomposition |
|---|---|---|
| Small | Single-concern, ≤2 files | Task only |
| Medium | Multi-file, single domain | Tech → Tasks |
| Large | Multi-domain, multi-stakeholder | Feature → Techs → Tasks |

---

## Handoff Contract

Every agent emits a structured handoff summary before yielding. `team-leader` verifies it before invoking the next agent.

```
{AGENT_NAME} COMPLETE
Plan file: .github/plans/<name>.md
{agent-specific fields}
Next agent: {name}
```

---

## Plan File Schema

Location: `.github/plans/<kebab-feature-name>.md`
Template: `.github/plans/_template.md`

| Section | Populated by | Always required |
|---|---|---|
| `## Request` | `kickoff-manager` | ✓ |
| `## Feature Breakdown` | `kickoff-manager` | ✓ |
| `## Architecture` | `architect-analyst` | ✓ |
| `## Implementation Plan` | `senior-dev-analyst` | ✓ |
| `## QA Strategy` | `quality-assurance-analyst` | ✓ |
| `## Decisions & Considerations` | All agents (append only) | ✓ |
| `## Status` | `team-leader` | ✓ |
| `## Implementation Progress` | `senior-dev` | During/after implementation |

### Status lifecycle

`Planning` → `Approved` → `In Progress` → `In Progress (paused)` → `Done`

---

## Cascade Rules

When the user requests a plan change, `team-leader` re-invokes agents downstream of the affected section:

| Change scope | Re-invoke |
|---|---|
| Architecture | `architect-analyst` → `senior-dev-analyst` → `quality-assurance-analyst` → `plan-writer` |
| Implementation | `senior-dev-analyst` → `quality-assurance-analyst` → `plan-writer` |
| QA strategy | `quality-assurance-analyst` → `plan-writer` |
| Scope/requirements | `kickoff-manager` → full pipeline |
