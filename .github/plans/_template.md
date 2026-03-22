# Plan: {Feature Name}

**Type:** `feature | bug`
**Status:** `Planning | Approved | In Progress | Done`
**Created:** {date}

---

## Request

> Original request verbatim.

### Clarifications

| Question | Answer |
|---|---|
| {question} | {answer} |

### Scope

- **In scope:** {what is included}
- **Out of scope:** {what is explicitly excluded}

---

## Feature Breakdown

> Populated by `kickoff-manager`. Depth (feature / tech / task) decided by `team-leader` based on complexity.

### Feature: {Feature Name}

#### Tech: {Tech Name}
*{One-sentence description of what this tech delivers and how it is testable.}*

#### Tech: {Tech Name}
*{…}*

---

## Architecture

> Populated by `architect-analyst`.

### Current State
*{Brief description of relevant existing architecture, if type is `feature` (modification) or `bug`.}*

### Proposed Structure

| Layer | Component | Responsibility |
|---|---|---|
| {layer} | {component} | {responsibility} |

### Key Decisions

| Decision | Rationale | Alternatives Considered |
|---|---|---|
| {decision} | {why} | {alternatives} |

### Dependencies & Risks

- {dependency or risk}

---

## Implementation Plan

> Populated by `senior-dev-analyst`. Each task is atomic, independently implementable, and verifiable.

### Tech: {Tech Name}

#### Task {N} — {Title}
- **What:** {one sentence}
- **Files:** {list of files to create or modify}
- **Why:** {justification}
- **Risk:** 🟢/🟡/🟠/🔴 — {what could go wrong}
- **Verify:** {test / build check / observable output}

---

## QA Strategy

> Populated by `quality-assurance-analyst`.

### Tech: {Tech Name}

| Test Type | Scope | Framework | Priority |
|---|---|---|---|
| Unit | {what to test} | {framework} | 🔴 High / 🟡 Med / 🟢 Low |
| Integration | {what to test} | {framework} | 🔴 High / 🟡 Med / 🟢 Low |
| E2E | {what to test} | {framework} | 🔴 High / 🟡 Med / 🟢 Low |

**Coverage targets:**
- Happy path: {description}
- Edge cases: {description}
- Error cases: {description}

---

## Decisions & Considerations

> Running log. Append entries; never delete.

| # | Phase | Decision | Rationale |
|---|---|---|---|
| 1 | Kickoff | {decision} | {rationale} |

---

## Implementation Progress

> Updated by `senior-dev` during implementation.

| Task | Status | PR / Commit | Notes |
|---|---|---|---|
| {Tech} / Task {N} | ⬜ Not started \| 🔄 In progress \| ✅ Done | {link} | {notes} |
