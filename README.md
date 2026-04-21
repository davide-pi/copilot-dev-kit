# 🤖 copilot-dev-kit

> A personal, curated toolkit to get the most out of GitHub Copilot.
> <br/>
> Custom `instructions`, `agents`, `skills`, and `prompts` refined through real daily dev work.

---

## Table of contents

- [🤖 copilot-dev-kit](#-copilot-dev-kit)
  - [Table of contents](#table-of-contents)
  - [🧠 Philosophy](#-philosophy)
  - [📦 What's inside](#-whats-inside)
    - [Agents](#agents)
    - [Instructions](#instructions)
    - [Prompts](#prompts)
    - [Skills](#skills)
  - [🚀 Getting started](#-getting-started)
  - [🔧 Compatibility](#-compatibility)
  - [🤝 Contributing](#-contributing)
  - [📄 License](#-license)

---

## 🧠 Philosophy

This repo is **NOT** a collection of generic AI tips.
<br/>
Everything here is something I actually use and find valuable during my daily work. The goal is to shape GitHub Copilot into a precise, context-aware assistant that fits my workflow, not a one-size-fits-all tool.

Key principles:

- **Specificity over generality** — prompts and instructions are scoped to real tasks, not abstract ideals
- **Iterative** — everything evolves as Copilot features and my workflow change
- **Opinionated** — this reflects my stack and preferences; fork freely and adapt (_suggestions are greatly appreciated_)

---

## 📦 What's inside

### Agents

Files into `.github/agents/` are the "workers" of your Copilot setup — they define custom agent personalities, system prompts, and tool access patterns for specialised workflows like code review, test generation, or any other specific task you want to delegate to an agent.

### Instructions

Files into `.github/instructions/` are the "rules" that guide agent (custom or not) behavior. They can be always-on or scoped (`**`) to specific file types (e.g. `**/*.cs`) based on the value of the `applyTo` frontmatter field.

### Prompts

Files into `.github/prompts/` are the "commands" that you can invoke directly from Copilot Chat as slash commands. They are ideal for lightweight, one-shot workflows that don't require a persistent agent session.

### Skills

Files into `.github/skills/` are the "abilities" that agents reference on demand. They are loaded based on the `Use when` pattern on the `description` frontmatter, which should describe the specific scenario or trigger for the skill.

---

## 🚀 Getting started

Clone or fork this repo, then copy the files you want into the `.github/` folder of your project. Copilot picks them up automatically (_no extension configuration required_).

> _**NOTE:** Be aware copying all files in the correct location if you want them to be discovered and used by Copilot. If you're only interested in specific parts, copy those selectively._
>

---

## 🔧 Compatibility

Designed for use with:

- [GitHub Copilot](https://github.com/features/copilot) (Individual / Business / Enterprise)
- VS Code with the Copilot Chat extension
- GitHub.com Copilot Chat (where supported)

---

## 🤝 Contributing

Found something useful? PRs and issues are welcome. If you're adapting this for a different stack or workflow, a fork is the right move, but suggestions are always welcome!

---

## 📄 License

MIT — use freely, attribution appreciated but not required. See [LICENSE](LICENSE) for details.
