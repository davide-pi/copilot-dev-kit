# Plan: Agent Pipeline Improvements

**Type:** `feature`
**Status:** `Done`
**Created:** 2026-03-22

---

## Request

> Analyze all `.github/` files and subfolders and improve them with: missing tools in agent frontmatter, anti-hallucination and specialization fixes, new shared skills for repeated patterns, context optimization (DRY violations, consistency), and template/prompt fixes.

### Clarifications

| Question | Answer |
|---|---|
| Scope | All `.github/agents/*.agent.md`, `.github/skills/*.SKILL.md`, `.github/instructions/**/*.md`, `.github/plans/_template.md`, `.github/prompts/pr-description.prompt.md` |
| New files allowed? | Yes — create `codebase-exploration.SKILL.md` and `git-workflows.SKILL.md` |
| Breaking changes? | None — all changes are additive (more tools, more constraints, new skills) |
| TypeScript instructions? | Out of scope for this iteration |

### Scope

- **In scope:** Agent tool lists, frontmatter handoffs/agents, anti-hallucination constraints, new shared skills, DRY elimination of inline stack detection, pipeline.SKILL.md consistency, copilot-authoring docs, template Italian fix, pr-description prompt improvement.
- **Out of scope:** Source code files, TypeScript/Python/Go instruction files, root README.md.

---

## Feature Breakdown

> Populated by `kickoff-manager`. Depth (feature / tech / task) decided by `team-leader` based on complexity.

### Feature: Agent Pipeline Completeness & Quality

#### Tech: New Shared Skills
*Create two new SKILL.md files (`codebase-exploration` and `git-workflows`) that extract duplicated inline logic from 5+ agents into single authoritative references, verifiable by checking that each agent referencing the skill no longer has inline duplicates.*

#### Tech: Agent Tool & Frontmatter Completeness
*Add missing tools (`todo`, `web`, `vscode/askQuestions`) to 6 agents and add `handoffs:` frontmatter entries where missing, verifiable by reading every agent's frontmatter and confirming no tool gap against the agent's protocol phases.*

#### Tech: Anti-Hallucination & Specialization
*Strengthen `senior-dev`, `plan-writer`, and `architect-analyst` with explicit guards: re-read Architecture before implementing, validate plan Status before finalizing, never invent package names. Verifiable by reviewing constraints sections.*

#### Tech: Context & Pipeline Consistency
*Fix `pipeline.SKILL.md` flow diagrams to match `team-leader.agent.md` (include plan-writer steps between each agent). Document `.agent.md` frontmatter fields in `copilot-authoring.instructions.md`. Verifiable by comparing diagrams side-by-side.*

#### Tech: Template & Prompt Fixes
*Fix Italian "modifica" comment in `_template.md`; add git log step to `pr-description.prompt.md`. Verifiable by searching for "modifica" and confirming absence.*

---

## Architecture

> Populated by `architect-analyst`.

### Current State
*All files are standalone `.md` files under `.github/`. Agents are `.agent.md` files with YAML frontmatter defining tools, name, description, and optionally agents/handoffs. Skills are `.SKILL.md` files with domain knowledge. Instructions are `.instructions.md` with `applyTo` frontmatter. No source code is modified.*

### Proposed Structure

| Layer | Component | Change |
|---|---|---|
| `.github/skills/` | `codebase-exploration.SKILL.md` | NEW — stack detection, layer mapping, test framework detection, naming conventions |
| `.github/skills/` | `git-workflows.SKILL.md` | NEW — blame, log, bisect, diff commands |
| `.github/skills/` | `pipeline.SKILL.md` | UPDATE — fix flow diagrams |
| `.github/agents/` | 8 agent files | UPDATE — tool lists, frontmatter, skill references, constraints |
| `.github/instructions/` | `copilot-authoring.instructions.md` | UPDATE — document agent frontmatter fields |
| `.github/plans/` | `_template.md` | UPDATE — fix Italian text |
| `.github/prompts/` | `pr-description.prompt.md` | UPDATE — add git log step |

### Key Decisions

| Decision | Rationale | Alternatives Considered |
|---|---|---|
| Extract stack detection to a shared skill | 5 agents duplicate the same 4-line detection block; single source of truth | Leave inline — rejected because divergence risk grows with each new stack |
| Add `todo` to all multi-phase agents | Protocol phases (Orient/Understand/Analyse/Output) need tracking; missing tool is a hard blocker | Optional — rejected because the protocol descriptions explicitly build todo lists |
| Add `handoffs:` to pipeline agents | VS Code surfaces handoff buttons for user navigation; currently only team-leader and senior-dev have them | Skip — rejected because discoverability is a UX requirement |
| Add `web` to investigation/planning agents | Bug investigator needs CVE lookup; senior-dev-analyst needs framework docs; neither can currently leave the workspace | Reject web tool — rejected because hallucination risk is higher without verified sources |

### Dependencies & Risks

- All agent changes are additive YAML / Markdown — zero risk of breaking existing behaviour.
- Skills must be created BEFORE agents reference them (tasks 1 and 2 are prerequisites for tasks 3–11).
- The `copilot-authoring.instructions.md` update is independent and can be done in any order.

---

## Implementation Plan

> Populated by `senior-dev-analyst`. Each task is atomic, independently implementable, and verifiable.

### Tech: New Shared Skills

#### Task 1 — Create codebase-exploration.SKILL.md
- **What:** Create `.github/skills/codebase-exploration.SKILL.md` with stack detection patterns, layer topology mapping, test framework detection, naming convention discovery, and entry point identification procedures.
- **Files:** `.github/skills/codebase-exploration.SKILL.md` (create new)
- **Why:** Five agents (architect-analyst, senior-dev-analyst, quality-assurance-analyst, quality-assurance-dev, bug-investigator) each contain identical inline stack detection logic searching for `*.csproj`, `package.json`, `pyproject.toml`, `go.mod`, `pom.xml`. A shared skill eliminates this duplication, ensures consistency, and allows adding new stacks in one place.
- **Risk:** 🟢 LOW — new file, no consumer exists yet
- **Verify:** File exists at `.github/skills/codebase-exploration.SKILL.md`; content covers stack manifest patterns, layer topology for .NET/Node/Python/Go, test framework detection table, naming convention discovery steps, entry point identification per stack.

#### Task 2 — Create git-workflows.SKILL.md
- **What:** Create `.github/skills/git-workflows.SKILL.md` with canonical git commands: file history (`git log --follow -p`), blame, diff patterns, commit inspection, and bisect workflow.
- **Files:** `.github/skills/git-workflows.SKILL.md` (create new)
- **Why:** The `bug-investigator` agent has inline git commands in Phase 2, and `code-review.agent.md` uses diff commands in Mode B — both independently without a shared reference. A skill prevents divergence and ensures both agents use consistent, correct git invocations.
- **Risk:** 🟢 LOW — new file
- **Verify:** File exists; covers `git log --follow -p`, `git blame`, `git bisect` workflow, diff commands, and commit inspection patterns.

---

### Tech: Agent Tool & Frontmatter Completeness

#### Task 3 — Update kickoff-manager.agent.md
- **What:** Add `- todo` to the `tools:` YAML list; add a `handoffs:` YAML block with one entry pointing to `architect-analyst`; add a sentence in Phase 0 Orient: "Read `.github/skills/codebase-exploration.SKILL.md` before searching the codebase."; add todo list usage in Phase 1 to track wizard questions answered.
- **Files:** `.github/agents/kickoff-manager.agent.md`
- **Why:** `kickoff-manager` runs a multi-phase wizard but has no `todo` tool for tracking phases. It searches the codebase in Phase 0 but duplicates inline discovery logic that now lives in the shared skill. `handoffs:` is missing, preventing VS Code sidebar navigation to the next agent.
- **Risk:** 🟢 LOW — additive YAML and prose changes only
- **Verify:** Frontmatter has `- todo` in tools; `handoffs:` block present with `architect-analyst`; Phase 0 references `codebase-exploration.SKILL.md`.

#### Task 4 — Update architect-analyst.agent.md
- **What:** Add `- todo` to `tools:`; add `handoffs:` block pointing to `senior-dev-analyst`; in Phase 0 replace "Detect the codebase stack: search for `*.csproj`..." with "Read `.github/skills/codebase-exploration.SKILL.md` for stack and layer detection."; add to Constraints: "Never invent package or library names not found in the codebase or `## Architecture` section.".
- **Files:** `.github/agents/architect-analyst.agent.md`
- **Why:** Multi-phase agent missing `todo`. Duplicates stack detection. No handoff button. Missing "don't invent packages" constraint is an anti-hallucination gap.
- **Risk:** 🟢 LOW — additive
- **Verify:** Frontmatter has `todo`; `handoffs:` to `senior-dev-analyst` present; Phase 0 references skill; new constraint present.

#### Task 5 — Update senior-dev-analyst.agent.md
- **What:** Add `- todo` and `- web` to `tools:`; add `handoffs:` to `quality-assurance-analyst`; in Phase 0 replace the inline stack detection sentence with "Read `.github/skills/codebase-exploration.SKILL.md` to identify the stack and layer structure."; add note: "Use `web` when a specific framework API behaviour is unclear before assuming it in tasks."
- **Files:** `.github/agents/senior-dev-analyst.agent.md`
- **Why:** Multi-phase planning agent lacks `todo` and `web`. Without `web`, it cannot consult framework documentation when planning tasks for unfamiliar APIs, increasing hallucination risk.
- **Risk:** 🟢 LOW — additive
- **Verify:** Tools include `todo` and `web`; `handoffs:` to `quality-assurance-analyst` in frontmatter; skill referenced in Phase 0.

#### Task 6 — Update quality-assurance-analyst.agent.md
- **What:** Add `- todo` and `- web` to `tools:`; add `handoffs:` to `plan-writer`; in Phase 0 replace "Detect the test framework already in use: search `*.csproj` for `xunit`/`nunit`/`mstest`; search `package.json` for `jest`/`vitest`/`@testing-library`." with "Read `.github/skills/codebase-exploration.SKILL.md` — use the Test Framework Detection table."; add to Constraints: "Never prescribe a test framework not already present in the codebase manifest files."
- **Files:** `.github/agents/quality-assurance-analyst.agent.md`
- **Why:** Same tool gaps as other planning agents. The test framework detection block is exactly what the new skill provides. The constraint prevents hallucinating a framework.
- **Risk:** 🟢 LOW — additive
- **Verify:** Tools include `todo` and `web`; `handoffs:` to `plan-writer`; skill referenced; constraint present.

#### Task 7 — Update quality-assurance-dev.agent.md
- **What:** Add `- todo` and `- vscode/askQuestions` to `tools:`; in Phase 0 replace "Detect test framework and test project structure (same approach as `test-writer`): Search `*.csproj` for `xunit`/`nunit`/`mstest`. Search `package.json` for `jest`/`vitest`/`@testing-library`." with "Read `.github/skills/codebase-exploration.SKILL.md` — use the Test Framework Detection section."; add note after "Build a todo list: one item per tech." → "Use the `todo` tool to track per-tech implementation progress."
- **Files:** `.github/agents/quality-assurance-dev.agent.md`
- **Why:** Phase 0 explicitly says "Build a todo list" but `todo` is not in the tools list — this is a hard contradiction. `vscode/askQuestions` is needed when test infrastructure setup is ambiguous.
- **Risk:** 🟢 LOW — additive
- **Verify:** Tools include `todo` and `vscode/askQuestions`; skill referenced; todo usage noted.

#### Task 8 — Update bug-investigator.agent.md
- **What:** Add `- web` to `tools:`; in Phase 0 add after the stack detection line: "Read `.github/skills/codebase-exploration.SKILL.md` before mapping the layer topology."; in Phase 2 after the git blame/log commands, add: "See `.github/skills/git-workflows.SKILL.md` for canonical git investigation commands."; add to Constraints (or Phase 2): "Use `web` to search for known CVEs, bug reports, or package changelogs when the symptom matches a known library issue."
- **Files:** `.github/agents/bug-investigator.agent.md`
- **Why:** `web` is absent, preventing CVE lookups and changelog research. The inline git commands in Phase 2 have a duplicate in the new skill — reference it for consistency. Stack detection duplicates the new skill.
- **Risk:** 🟢 LOW — additive
- **Verify:** `web` in tools; both skills referenced at appropriate points; `web` usage note present.

#### Task 9 — Update code-review.agent.md
- **What:** In the `## Understand` section's Mode B diff command block, add after the git diff line: "See `.github/skills/git-workflows.SKILL.md` for canonical diff and log commands."; add a new `## TypeScript / JavaScript` section after `## Analyse`: "For JS/TS files: apply ESLint/Prettier conventions found in `.eslintrc.js`/`.prettierrc`. Flag `any` type usage, missing return types, and unsafe `as` casts as [IMPORTANT]. Flag console.log left in production code as [IMPORTANT]. No language-specific `.instructions.md` exists for TS/JS — apply the generic severity model from `code-review.instructions.md`."
- **Files:** `.github/agents/code-review.agent.md`
- **Why:** Mode B uses ad-hoc diff commands when a skill now exists. The agent description says it reviews TypeScript and JavaScript, but there are zero TS/JS-specific guidelines — reviewers have no guidance on what to flag.
- **Risk:** 🟢 LOW — additive
- **Verify:** `git-workflows.SKILL.md` reference present in Mode B; TypeScript/JavaScript section added with specific findings guidance.

---

### Tech: Anti-Hallucination & Specialization

#### Task 10 — Update senior-dev.agent.md
- **What:** Add `- web` to `tools:`; in Phase 0 after "Build a todo list from `## Implementation Plan`" add: "Read `## Architecture` in full before any implementation begins. Re-read before each task with risk 🟠 HIGH or 🔴 CRITICAL."; add to Constraints: "Never install or import a package not mentioned in `## Architecture` — if a new dependency is needed, ask the user before proceeding."
- **Files:** `.github/agents/senior-dev.agent.md`
- **Why:** Without reading Architecture before implementation, senior-dev risks implementing components that violate the approved structural design. The package constraint prevents scope creep. `web` enables looking up official framework docs during implementation.
- **Risk:** 🟡 MEDIUM — modifies Phase 0 protocol steps
- **Verify:** `web` in tools; Architecture re-read step in Phase 0; package constraint in Constraints section.

#### Task 11 — Update plan-writer.agent.md
- **What:** In `finalize` mode Phase 0, add after "Read the plan file in full.": "Check `## Status` — if it is `Approved`, `In Progress`, or `Done`, stop and report to `team-leader`: 'Plan is already {status} — cannot re-finalize without explicit reset.'"; add `## Status` as a mandatory section in the verification checklist: "- `## Status` must be `Planning`"; in Phase 1 validation add for `## Implementation Plan`: "All tasks have What / Files / Why / Risk / Verify (five fields required — reject if any is missing)."
- **Files:** `.github/agents/plan-writer.agent.md`
- **Why:** A plan that is already Approved could be accidentally re-finalized, wiping progress. Status field is required but not in the validation checklist. The five-field task validation is described qualitatively but not explicitly enforced per field.
- **Risk:** 🟡 MEDIUM — modifies finalize mode validation logic
- **Verify:** Guard against re-finalization of non-Planning plans; `## Status` in validation checklist; five-field check explicit.

---

### Tech: Context & Pipeline Consistency

#### Task 12 — Update pipeline.SKILL.md
- **What:** Replace the simplified Feature Flow and Bug Flow diagrams with the full diagrams from `team-leader.agent.md` that include `plan-writer(section)` steps after each agent; update the handoff contract example to show the full `plan-writer` interleaving.
- **Files:** `.github/skills/pipeline.SKILL.md`
- **Why:** The current skill diagrams show: `kickoff-manager → architect-analyst → senior-dev-analyst → quality-assurance-analyst → plan-writer` — which is wrong. The actual flow has `plan-writer(section)` invoked after EACH agent. Agents reading this skill get an inaccurate pipeline model, leading to incorrect handoff assumptions.
- **Risk:** 🟢 LOW — documentation only
- **Verify:** Feature Flow diagram matches `team-leader.agent.md` exactly; Bug Flow matches; no simplified version remains.

#### Task 13 — Update copilot-authoring.instructions.md
- **What:** Add a new subsection under `## File-Type Rules` titled `### .agent.md` documenting these frontmatter fields: `name:` (machine identifier), `description:` (one-line purpose), `tools:` (list of allowed tools: `read`, `edit`, `execute`, `search`, `todo`, `agent`, `web`, `vscode/askQuestions`), `agents:` (list of agent names this agent may invoke), `handoffs:` (list of VS Code sidebar action buttons with `label`, `agent`, `prompt`, `send` fields), `user-invocable: false` (orchestrator-only agents not shown in user menus); add note: "SKILL.md files do not require frontmatter. If provided, only `name:` and `description:` are used."
- **Files:** `.github/instructions/copilot-authoring.instructions.md`
- **Why:** The authoring guide covers all file types EXCEPT the `.agent.md` frontmatter fields. Any author creating a new agent has no reference for `handoffs:`, `agents:`, or `user-invocable:`. This is the most used format in this repo and the least documented.
- **Risk:** 🟢 LOW — additive documentation
- **Verify:** `.agent.md` section present; all six frontmatter fields documented with their purpose; `user-invocable:` and `handoffs:` format explained.

---

### Tech: Template & Prompt Fixes

#### Task 14 — Fix _template.md
- **What:** In `## Architecture` section's "Current State" comment, change "type is `modifica` or `bug`" to "type is `feature` (modification) or `bug`". Ensure the plan header has `**Status:** Planning` as a visible field (not just a Markdown comment).
- **Files:** `.github/plans/_template.md`
- **Why:** The word "modifica" is Italian and inconsistent with the English-only convention throughout the file. It causes confusion for any English-speaking developer reading the template.
- **Risk:** 🟢 LOW — template text fix only
- **Verify:** No Italian text remains in the template; grep for "modifica" returns no results.

#### Task 15 — Update pr-description.prompt.md
- **What:** Insert a new step between Step 1 and Step 2: "**Step 1b – Collect commit messages.** Run `git log origin/main...HEAD --oneline` to get commit messages. These provide the 'why' context that file diffs alone cannot reveal. Use them to populate the `## Context` section accurately."
- **Files:** `.github/prompts/pr-description.prompt.md`
- **Why:** Currently the prompt generates the Context section purely from the file diff. Commit messages explain WHY changes were made — information the diff does not contain. Without them, the Context section tends to describe WHAT changed (duplicating Changes) rather than WHY.
- **Risk:** 🟢 LOW — additive step
- **Verify:** Prompt contains git log command; Context section guidance references commit messages as source material.

---

## QA Strategy

> Populated by `quality-assurance-analyst`.

### Tech: New Shared Skills

| Test Type | Scope / What to verify | Framework | Priority |
|---|---|---|---|
| Manual | File exists at correct path; content covers all documented stacks | File inspection | 🔴 |
| Manual | No agent still has inline stack detection after Tech 3 updates | grep search | 🟡 |

**Test data:** N/A — documentation files

**Coverage:**
- Happy path: Both skill files created with correct content
- Edge cases: Skills remain coherent if a new stack is added later (open-ended design)
- Error cases: Skill file path mismatch — verified by reading each referencing agent

### Tech: Agent Tool & Frontmatter Completeness

| Test Type | Scope / What to verify | Framework | Priority |
|---|---|---|---|
| Manual | Every updated agent's frontmatter has correct tools list | YAML inspection | 🔴 |
| Manual | Every updated agent has `handoffs:` where applicable | YAML inspection | 🟡 |
| Manual | Skill references are correct file paths | File inspection | 🟡 |

**Test data:** N/A

**Coverage:**
- Happy path: All 9 agent files updated correctly with no YAML syntax errors
- Edge cases: agent with `user-invocable: false` does not need `handoffs:` (plan-writer, kickoff-manager)
- Error cases: Missing `handoffs:` means VS Code won't show navigation buttons — verified visually

### Tech: Anti-Hallucination & Specialization

| Test Type | Scope / What to verify | Framework | Priority |
|---|---|---|---|
| Manual | senior-dev Phase 0 includes Architecture re-read step | File inspection | 🔴 |
| Manual | plan-writer finalize mode has Status guard | File inspection | 🔴 |
| Manual | architect-analyst has "never invent packages" constraint | File inspection | 🟡 |

**Test data:** N/A

**Coverage:**
- Happy path: All three agents updated with correct constraints
- Error cases: Constraint wording must be clear and unambiguous — reviewed manually

### Tech: Context & Pipeline Consistency

| Test Type | Scope / What to verify | Framework | Priority |
|---|---|---|---|
| Manual | pipeline.SKILL.md flow diagrams match team-leader.agent.md | Side-by-side comparison | 🔴 |
| Manual | copilot-authoring.instructions.md has .agent.md section | File inspection | 🟡 |

**Test data:** N/A

**Coverage:**
- Happy path: Diagrams are identical; authoring docs are complete
- Error cases: Any diagram difference is a failure; section must cover all 6 frontmatter fields

### Tech: Template & Prompt Fixes

| Test Type | Scope / What to verify | Framework | Priority |
|---|---|---|---|
| Manual | No Italian text in _template.md | grep "modifica" | 🔴 |
| Manual | pr-description.prompt.md has git log step | File inspection | 🟡 |

**Test data:** N/A

**Coverage:**
- Happy path: Both files fixed correctly
- Error cases: Any remaining Italian text fails the check

---

## Decisions & Considerations

> Running log. Append entries; never delete.

| # | Phase | Decision | Rationale |
|---|---|---|---|
| 1 | Architecture | Extract inline stack detection to shared skill | 5 agents had identical 4-line detection blocks — DRY violation |

---

## Implementation Progress

> Updated by `senior-dev` during implementation.

| Task | Status | PR / Commit | Notes |
|---|---|---|---|
| New Shared Skills / Task 1 — Create codebase-exploration.SKILL.md | ✅ Done | — | — |
| New Shared Skills / Task 2 — Create git-workflows.SKILL.md | ✅ Done | — | — |
| Agent Tool & Frontmatter Completeness / Task 3 — Update kickoff-manager.agent.md | ✅ Done | — | — |
| Agent Tool & Frontmatter Completeness / Task 4 — Update architect-analyst.agent.md | ✅ Done | — | — |
| Agent Tool & Frontmatter Completeness / Task 5 — Update senior-dev-analyst.agent.md | ✅ Done | — | — |
| Agent Tool & Frontmatter Completeness / Task 6 — Update quality-assurance-analyst.agent.md | ✅ Done | — | — |
| Agent Tool & Frontmatter Completeness / Task 7 — Update quality-assurance-dev.agent.md | ✅ Done | — | — |
| Agent Tool & Frontmatter Completeness / Task 8 — Update bug-investigator.agent.md | ✅ Done | — | — |
| Agent Tool & Frontmatter Completeness / Task 9 — Update code-review.agent.md | ✅ Done | — | — |
| Anti-Hallucination & Specialization / Task 10 — Update senior-dev.agent.md | ✅ Done | — | — |
| Anti-Hallucination & Specialization / Task 11 — Update plan-writer.agent.md | ✅ Done | — | — |
| Context & Pipeline Consistency / Task 12 — Update pipeline.SKILL.md | ✅ Done | — | — |
| Context & Pipeline Consistency / Task 13 — Update copilot-authoring.instructions.md | ✅ Done | — | — |
| Template & Prompt Fixes / Task 14 — Fix _template.md | ✅ Done | — | — |
| Template & Prompt Fixes / Task 15 — Update pr-description.prompt.md | ✅ Done | — | — |
