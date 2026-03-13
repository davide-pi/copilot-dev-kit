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
        ├── code-review.prompt.md                   ← structured code review slash command
        ├── pr-description.prompt.md                ← PR description from git diff
        ├── commit-message.prompt.md                ← Conventional Commits message
        ├── test-writer.prompt.md                   ← unit/integration test generation
        ├── refactor.prompt.md                      ← guided step-by-step refactoring
        ├── doc-generator.prompt.md                 ← XML docs / TSDoc / JSDoc / README
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

### Skills

Copy `SKILL.md` files into `.github/skills/`. Skills package domain knowledge that agents can reference — Copilot will surface them as available capabilities when composing agent workflows.

### Prompts

Copy `.prompt.md` files into `.github/prompts/`. They appear as `/` slash commands in Copilot Chat. Each file can include a description and input variables, making them reusable across projects.

---

## 🗂️ Highlights

### Instructions

| Name | `applyTo` | What it does |
|---|---|---|
| **copilot-instructions.md** | always on | Clean Code, SOLID, DRY/KISS/YAGNI, Clean Architecture, testing & security mindset |
| **planning.instructions.md** | always on | Decomposes work into atomic steps with dependency schema and implementation-ordered summary |
| **csharp.instructions.md** | `**/*.cs` | Naming, async/await, nullable, records, DI, ASP.NET Core API conventions |
| **javascript-typescript.instructions.md** | `**/*.{js,ts,jsx,tsx}` | TypeScript strict mode, React hooks, Angular signals/OnPush, import order |
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
| **code-review** | Structured code review — full file or diff, C# and JS/TS |
| **pr-description** | Generates PR description (context, changes, breaking changes, checklist) from `git diff` |
| **commit-message** | Conventional Commits message from staged changes |
| **test-writer** | Generates unit/integration tests with coverage rationale and edge cases |
| **refactor** | Guided step-by-step refactoring with rationale for each atomic change |
| **doc-generator** | XML docs / TSDoc / JSDoc and module README generation |
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
