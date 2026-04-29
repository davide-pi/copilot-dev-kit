# copilot-dev-kit

> A curated toolkit of custom `instructions`, `skills`, and `prompts` for GitHub Copilot, refined through daily use.

## Philosophy

Everything here is something I actually use. The goal is to shape Copilot into a precise, context-aware assistant — not a generic one.

- **Specificity over generality** — scoped to real tasks, not abstract ideals
- **Iterative** — evolves with Copilot features and workflow changes
- **Opinionated** — reflects my stack; fork and adapt freely

## What's Inside

### Instructions (`.github/instructions/`)

Markdown files declaring rules for agent behavior. Always-on or scoped to file types via `applyTo` frontmatter (e.g. `**/*.cs`).

### Prompts (`.github/prompts/`)

Slash-command `.prompt.md` files for one-shot workflows invoked directly from Copilot Chat.

### Skills (`.github/skills/`)

On-demand capability files (`SKILL.md`) loaded when an agent matches the `description` trigger in frontmatter.

## Getting Started

Clone or fork, then copy the desired files into your project's `.github/` folder. Copilot discovers them automatically.

> Only the files in the correct location are discovered. Copy selectively if you don't need everything.

## Compatibility

- [GitHub Copilot](https://github.com/features/copilot) (Individual / Business / Enterprise)
- VS Code with Copilot Chat extension
- GitHub.com Copilot Chat (where supported)

## Contributing

PRs and issues welcome. For a different stack, fork and adapt — suggestions always appreciated.

## License

MIT — see [LICENSE](LICENSE).
