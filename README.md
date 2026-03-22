# 🤖 copilot-dev-kit

> A personal, curated toolkit to get the most out of GitHub Copilot — instructions, custom agents, skills, and prompt templates refined through real daily dev work.

---

## Table of contents

- [🤖 copilot-dev-kit](#-copilot-dev-kit)
  - [Table of contents](#table-of-contents)
  - [📦 What's inside](#-whats-inside)
  - [🧠 Philosophy](#-philosophy)
  - [🚀 Getting started](#-getting-started)
    - [Recommended project layout](#recommended-project-layout)
    - [Custom instructions](#custom-instructions)
    - [Agents](#agents)
    - [Skills](#skills)
    - [Prompts](#prompts)
  - [🗂️ Highlights](#️-highlights)
    - [Instructions](#instructions)
    - [Agents](#agents-1)
    - [Skills](#skills-1)
    - [Prompts](#prompts-1)
  - [🔧 Compatibility](#-compatibility)
  - [🤝 Contributing](#-contributing)
  - [📄 License](#-license)
  - [📦 What's inside](#-whats-inside-1)
  - [🧠 Philosophy](#-philosophy-1)
  - [🚀 Getting started](#-getting-started-1)
    - [Recommended project layout](#recommended-project-layout-1)
    - [Custom instructions](#custom-instructions-1)
    - [Agents](#agents-2)
    - [Skills](#skills-2)
    - [Prompts](#prompts-2)
  - [🗂️ Highlights](#️-highlights-1)
    - [Instructions](#instructions-1)
    - [Agents](#agents-3)
    - [Skills](#skills-3)
    - [Prompts](#prompts-3)
  - [🔧 Compatibility](#-compatibility-1)
  - [🤝 Contributing](#-contributing-1)
  - [📄 License](#-license-1)

---

## 📦 What's inside

| Folder | Description |
|---|---|
| `.github/instructions/` | Reusable `.instructions.md` files and the project-level `copilot-instructions.md` |
| `.github/agents/` | Custom agent definitions (`.agent.md`) — a full multi-agent development pipeline |
| `.github/skills/` | `SKILL.md` definitions that package domain knowledge loaded on demand by agents |
| `.github/prompts/` | Reusable `.prompt.md` templates for common dev tasks — use as slash commands in Copilot Chat |
| `.github/plans/` | Plan files produced by the agent pipeline; `_template.md` is the canonical template |

---

## 🧠 Philosophy

This repo is **not** a collection of generic AI tips. Everything here is something I actually use and find valuable. The goal is to shape GitHub Copilot into a precise, context-aware assistant that fits my workflow — not a one-size-fits-all tool.

Key principles:

- **Specificity over generality** — prompts and instructions are scoped to real tasks, not abstract ideals
- **Iterative** — everything evolves as Copilot features and my workflow change
- **Opinionated** — this reflects my stack and preferences; fork freely and adapt (_suggestions are greatly appreciated_)

---

## 🚀 Getting started

Clone or fork this repo, then copy the files you want into your project's `.github/` folder. Copilot picks them up automatically — no extension configuration required.

### Recommended project layout

```
your-project/
└── .github/
    ├── copilot-instructions.md                      ← always-on, language-agnostic rules
    ├── instructions/
    │   ├── planning.instructions.md                 ← planning & decomposition rules
    │   ├── code-review.instructions.md              ← severity model & output format (all languages)
    │   ├── pipeline.instructions.md                 ← CI/CD pipeline conventions (all platforms)
    │   ├── copilot-authoring.instructions.md        ← rules for authoring agent/skill/instruction files
    │   └── c-sharp/
    │       ├── csharp.instructions.md               ← C# / ASP.NET Core naming & code conventions
    │       └── code-review-csharp.instructions.md   ← severity assignments for C#-specific violations
    ├── agents/
    │   ├── team-leader.agent.md                     ← orchestrator: routes requests through the pipeline
    │   ├── kickoff-manager.agent.md                 ← requirements elicitation & feature decomposition
    │   ├── architect-analyst.agent.md               ← architectural design & component proposal
    │   ├── senior-dev-analyst.agent.md              ← atomic task decomposition & implementation plan
    │   ├── senior-dev.agent.md                      ← task-by-task implementation with checkpoints
    │   ├── quality-assurance-analyst.agent.md       ← test strategy planning
    │   ├── quality-assurance-dev.agent.md           ← test implementation
    │   ├── plan-writer.agent.md                     ← plan file creation, section writing & finalization
    │   ├── bug-investigator.agent.md                ← root-cause analysis & fix recommendation
    │   ├── code-review.agent.md                     ← structured code review with severity model
    │   └── test-writer.agent.md                     ← test generation (C# / xUnit / NUnit)
    ├── skills/
    │   ├── clean-architecture.SKILL.md              ← Clean Architecture knowledge package
    │   ├── testing.SKILL.md                         ← testing strategy & pattern knowledge
    │   ├── codebase-exploration.SKILL.md            ← stack detection, layer mapping, entry points
    │   ├── git-workflows.SKILL.md                   ← blame, log, diff, bisect commands
    │   └── pipeline.SKILL.md                        ← agent pipeline flows, handoff contracts, plan schema
    ├── plans/
    │   └── _template.md                             ← canonical plan file template
    └── prompts/
        └── pr-description.prompt.md                 ← PR description from git diff + commit messages
```

### Custom instructions

`copilot-instructions.md` is loaded for every Copilot interaction in the repo. Use it for always-on rules (coding style, conventions, stack preferences).

```bash
# macOS / Linux / PowerShell Core
cp .github/copilot-instructions.md your-project/.github/copilot-instructions.md

# Windows (PowerShell)
Copy-Item .github\copilot-instructions.md your-project\.github\copilot-instructions.md
```

For scoped rules that only apply to specific file types (e.g. `**/*.cs`), use `.instructions.md` files with an `applyTo` frontmatter field in `.github/instructions/`.

### Agents

Copy `.agent.md` files into `.github/agents/`. In VS Code, open Copilot Chat and select the agent from the mode picker (the dropdown next to the chat input). Each agent file defines its name, description, available tools, and system prompt.

The recommended entry point is **`team-leader`** — it orchestrates the full pipeline and delegates to every other agent automatically.

### Skills

Copy `SKILL.md` files into `.github/skills/`. Skills package domain knowledge that agents reference on demand — they are loaded only when an agent explicitly reads them, keeping always-on context lean.

### Prompts

Copy `.prompt.md` files into `.github/prompts/`. They appear as slash commands in Copilot Chat. Only lightweight, one-shot workflows that don't require a persistent agent session live here.

---

## 🗂️ Highlights

### Instructions

| Name | `applyTo` | What it does |
|---|---|---|
| **copilot-instructions.md** | always on | Clean Code, SOLID, DRY/KISS/YAGNI, Clean Architecture, testing & security mindset |
| **planning.instructions.md** | always on | Decomposes work into atomic steps with dependency schema and implementation-ordered summary |
| **code-review.instructions.md** | always on | Severity model (CRITICAL / IMPORTANT / SUGGESTION) and finding output format |
| **copilot-authoring.instructions.md** | `.github/**/*.md` | Rules for authoring agent, skill, instruction, and prompt files |
| **pipeline.instructions.md** | `**/*.{yml,yaml}` | CI/CD conventions: scripts over platform tasks, reusable templates, secrets, fail-fast, security |
| **csharp.instructions.md** | `**/*.cs` | Naming, async/await, nullable, records, DI, ASP.NET Core API conventions |
| **code-review-csharp.instructions.md** | `**/*.cs` | Maps C# violations (async, DI lifetimes, nullable, LINQ, security) to severity levels |

### Agents

The agents form a sequential pipeline, orchestrated by `team-leader`. For most requests, invoke `team-leader` and it will delegate automatically.

| Name | Role in pipeline |
|---|---|
| **team-leader** | Orchestrator — classifies the request, sequences agents, manages the plan lifecycle and approval gates |
| **kickoff-manager** | Requirements elicitation — interviews the user, classifies `feature`/`bug`, decomposes into features & techs |
| **architect-analyst** | Architecture design — maps the existing codebase and proposes the structural solution |
| **senior-dev-analyst** | Task planner — decomposes techs into atomic, independently verifiable implementation tasks |
| **plan-writer** | Plan file manager — creates, updates, and finalizes `.github/plans/*.md` files |
| **senior-dev** | Implementer — executes one task at a time with user checkpoints; requires an Approved plan |
| **quality-assurance-analyst** | Test strategist — defines the test pyramid for every tech before any code is written |
| **quality-assurance-dev** | Test implementer — generates tests per the QA strategy via the `test-writer` agent |
| **test-writer** | Test generator — produces compilable C# test files (xUnit/NUnit) following AAA |
| **bug-investigator** | Bug analyst — traces root causes through code, git history, and logs; never modifies code |
| **code-review** | Reviewer — structured code review with severity labels and Before/After snippets |

### Skills

| Name | What it packages |
|---|---|
| **clean-architecture** | Dependency Rule, layer responsibilities, Ports & Adapters, common violations, project structure |
| **testing** | Test pyramid, Microsoft L0–L4 levels, AAA pattern, test doubles, naming, coverage philosophy |
| **codebase-exploration** | Stack detection, layer topology mapping, test framework detection, naming conventions, entry points |
| **git-workflows** | Canonical git commands: blame, log, diff, bisect, stash — used by bug-investigator and code-review agents |
| **pipeline** | Agent pipeline flows, handoff contracts, plan file schema, cascade rules |

### Prompts

| Name | What it does |
|---|---|
| **pr-description** | Generates a structured PR description (context, changes, breaking changes, checklist) from `git diff` + commit log |

---

## 🔧 Compatibility

Designed for use with:

- [GitHub Copilot](https://github.com/features/copilot) (Individual / Business / Enterprise)
- VS Code with the Copilot Chat extension
- GitHub.com Copilot Chat (where supported)

---

## 🤝 Contributing

Found something useful? PRs and issues are welcome. If you're adapting this for a different stack or workflow, a fork is the right move — keep your kit yours.

---

## 📄 License

MIT — use freely, attribution appreciated but not required.


---

## 📦 What's inside

| Folder | Description |
|---|---|
| `instructions/` | Reusable `.instructions.md` files and project-level `copilot-instructions.md` templates |
| `agents/` | Custom agent definitions (`.agent.md`) for specialised workflows like code review or PR generation |
| `.github/skills/` | `SKILL.md` definitions that package domain knowledge and tool access patterns |
| `prompts/` | Reusable `.prompt.md` templates for common dev tasks — use directly as slash commands in Copilot Chat |

---

## 🧠 Philosophy

This repo is **not** a collection of generic AI tips. Everything here is something I actually use and find valuable. The goal is to shape GitHub Copilot into a precise, context-aware assistant that fits my workflow — not a one-size-fits-all tool.

Key principles:

- **Specificity over generality** — prompts and instructions are scoped to real tasks, not abstract ideals
- **Iterative** — everything evolves as Copilot features and my workflow change
- **Opinionated** — this reflects my stack and preferences; fork freely and adapt (_suggestions are greatly appreciated_)

---

## 🚀 Getting started

Clone or fork this repo, then copy the files you want into your project's `.github/` folder. Copilot picks them up automatically — no extension configuration required.

### Recommended project layout

```
your-project/
└── .github/
    ├── copilot-instructions.md                     ← always-on, language-agnostic rules
    ├── instructions/
    │   ├── planning.instructions.md                ← planning & decomposition rules
    │   ├── csharp.instructions.md                  ← C# / ASP.NET Core conventions
    │   ├── javascript-typescript.instructions.md   ← JS/TS/React/Angular conventions
    │   ├── pipeline.instructions.md                ← CI/CD pipeline conventions (all platforms)
    │   ├── code-review.instructions.md             ← severity model & output format (all languages)
    │   ├── code-review-csharp.instructions.md      ← severity assignments for C#-specific violations
    │   └── code-review-js-ts.instructions.md       ← severity assignments for JS/TS-specific violations
    ├── agents/
    │   ├── code-review.agent.md                    ← multi-language code review agent
    │   ├── refactor.agent.md                       ← step-by-step refactoring agent
    │   ├── test-writer.agent.md                    ← test generation agent
    │   └── doc-generator.agent.md                  ← documentation generation agent
    ├── skills/
    │   ├── clean-architecture.SKILL.md             ← Clean Architecture knowledge package
    │   └── testing.SKILL.md                        ← testing strategy knowledge package
    └── prompts/
        ├── pr-description.prompt.md                ← PR description from git diff
        └── breaking-change-analysis.prompt.md      ← breaking change detection
```

### Custom instructions

`copilot-instructions.md` is loaded for every Copilot interaction in the repo. Use it for always-on rules (coding style, conventions, stack preferences).

```bash
# macOS / Linux / PowerShell Core
cp instructions/copilot-instructions.md .github/copilot-instructions.md

# Windows (PowerShell)
Copy-Item instructions\copilot-instructions.md .github\copilot-instructions.md
```

For scoped rules that only apply to specific file types (e.g. `**/*.cs`), use `.instructions.md` files with an `applyTo` frontmatter field in `.github/instructions/`.

### Agents

Copy `.agent.md` files into `.github/agents/`. In VS Code, open Copilot Chat and select the agent from the mode picker (the dropdown next to the chat input). Each agent file defines its name, description, available tools, and system prompt.

Code review, refactoring, test generation, and documentation workflows are fully covered by agents — no separate prompt files are needed for these.

### Skills

Copy `SKILL.md` files into `.github/skills/`. Skills package domain knowledge that agents can reference — Copilot will surface them as available capabilities when composing agent workflows.

### Prompts

Copy `.prompt.md` files into `.github/prompts/`. They appear as slash commands in Copilot Chat. Only lightweight, one-shot workflows that don't require a persistent agent session live here.

---

## 🗂️ Highlights

### Instructions

| Name | `applyTo` | What it does |
|---|---|---|
| **copilot-instructions.md** | always on | Clean Code, SOLID, DRY/KISS/YAGNI, Clean Architecture, testing & security mindset |
| **planning.instructions.md** | always on | Decomposes work into atomic steps with dependency schema and implementation-ordered summary |
| **csharp.instructions.md** | `**/*.cs` | Naming, async/await, nullable, records, DI, ASP.NET Core API conventions |
| **javascript-typescript.instructions.md** | `**/*.{js,ts,jsx,tsx}` | TypeScript strict mode, React hooks, Angular signals/OnPush, import order |
| **pipeline.instructions.md** | `**/*.{yml,yaml}` | CI/CD conventions: scripts over platform tasks, reusable templates, readability, secrets, fail-fast, security |
| **code-review.instructions.md** | always on | Severity model (CRITICAL / IMPORTANT / SUGGESTION) and finding output format |
| **code-review-csharp.instructions.md** | `**/*.cs` | Maps C# violations (async, DI lifetimes, nullable, LINQ, security) to severity levels; delegates conventions to `csharp.instructions.md` |
| **code-review-js-ts.instructions.md** | `**/*.{js,ts,jsx,tsx}` | Maps JS/TS violations (strict TS, React hooks, Angular, XSS) to severity levels; delegates conventions to `javascript-typescript.instructions.md` |

### Agents

| Name | What it does |
|---|---|
| **code-review** | Multi-language structured review using all three code-review instruction files |
| **refactor** | Atomic step-by-step refactoring with Before/After and behavioural equivalence guarantee |
| **test-writer** | Analyses coverage gaps and generates compilable tests (xUnit/NUnit/Jest/Vitest) |
| **doc-generator** | Generates XML docs, TSDoc/JSDoc, and module-level READMEs for existing code |

### Skills

| Name | What it packages |
|---|---|
| **clean-architecture** | Dependency Rule, layer responsibilities, Ports & Adapters, common violations, project structure |
| **testing** | Test pyramid, Microsoft L0–L4 test levels, AAA pattern, test doubles, naming conventions, coverage philosophy |

### Prompts

| Name | What it does |
|---|---|
| **pr-description** | Generates PR description (context, changes, breaking changes, checklist) from `git diff` |
| **breaking-change-analysis** | Identifies breaking changes in a diff with impact and migration path |

---

## 🔧 Compatibility

Designed for use with:

- [GitHub Copilot](https://github.com/features/copilot) (Individual / Business / Enterprise)
- VS Code with the Copilot Chat extension
- GitHub.com Copilot Chat (where supported)

---

## 🤝 Contributing

Found something useful? PRs and issues are welcome. If you're adapting this for a different stack or workflow, a fork is the right move — keep your kit yours.

---

## 📄 License

MIT — use freely, attribution appreciated but not required.
