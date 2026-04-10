# copilot-dev-kit
Personal toolkit of agents, instructions, prompts and skills to improve my usage of GitHub Copilot across my daily development workflow.
# 🤖 copilot-dev-kit

> A personal, curated toolkit to get the most out of GitHub Copilot — instructions, custom agents, skills, and prompt templates refined through real daily dev work.

---

## Table of contents

- [copilot-dev-kit](#copilot-dev-kit)
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
    - [Skills](#skills-1)
  - [🔧 Compatibility](#-compatibility)
  - [🤝 Contributing](#-contributing)
  - [📄 License](#-license)

---

## 📦 What's inside

| Folder | Description |
|---|---|
| `.github/instructions/` | Reusable `.instructions.md` files — always-on rules scoped by file pattern |
| `.github/agents/` | Custom agent definitions (`.agent.md`) for specialised workflows |
| `.github/skills/` | `SKILL.md` definitions that package domain knowledge loaded on demand by agents |
| `.github/prompts/` | Reusable `.prompt.md` templates for common dev tasks — use as slash commands in Copilot Chat |

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
    ├── instructions/
    │   └── voice.instructions.md        ← feedback tone & communication style
    ├── agents/
    ├── skills/
    │   └── write-a-skill/
    │       └── SKILL.md                 ← how to create new skills
    └── prompts/
```

### Custom instructions

Copy `.instructions.md` files into `.github/instructions/`. Copilot picks them up automatically. Use the `applyTo` frontmatter to scope rules to specific file patterns — omit it (or set `applyTo: '**'`) to apply globally.

### Agents

Copy `.agent.md` files into `.github/agents/`. In VS Code, open Copilot Chat and select the agent from the mode picker (the dropdown next to the chat input). Each agent file defines its name, description, available tools, and system prompt.

### Skills

Copy `SKILL.md` files into `.github/skills/<skill-name>/`. Skills package domain knowledge that agents load on demand by calling `read_file` — they never inflate the always-on context window.

### Prompts

Copy `.prompt.md` files into `.github/prompts/`. They surface as slash commands in Copilot Chat. Keep these to lightweight, one-shot workflows — anything requiring multi-step state belongs in an agent instead.

---

## 🗂️ Highlights

### Instructions

| Name | `applyTo` | What it does |
|---|---|---|
| **voice.instructions.md** | `**` | Enforces direct, critical feedback — no filler, exact problem + reason + fix |

### Skills

| Name | What it packages |
|---|---|
| **write-a-skill** | Process and templates for creating new `SKILL.md` files with proper structure, progressive disclosure, and bundled resources |

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
