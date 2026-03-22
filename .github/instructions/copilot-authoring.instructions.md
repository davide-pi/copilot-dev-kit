---
applyTo: ".github/**/*.md"
---

# Copilot File Authoring

Rules for creating and editing `copilot-instructions.md`, `.instructions.md`, `.prompt.md`, `.SKILL.md`, and `.agent.md` files.

---

## Token Economy

- State directives, not rationale. Omit explanations a senior dev already knows.
- Tables > prose for structured data. Bullets > paragraphs. One idea per line.
- No preambles ("In order to…"), no filler ("Note that…"), no trailing summaries.
- Inline references (`See <file>`) instead of restating content from another loaded file.
- Use 🔴/🟡/🟢 for severity; omit legend when it's defined in `copilot-instructions.md`.
- Omit any section that has nothing to say. Never add placeholder content.

---

## Anti-Hallucination

- Read the target file fully before editing.
- Verify every referenced file path exists in the workspace before writing it.
- Never invent tool names, CLI flags, or API signatures without confirmation.
- Mark optional output sections explicitly: `(omit if none)`.
- When unsure of a file location, search first; never guess.

---

## When to Use Each File Type

Choose the type that best fits the need — there is no single correct answer.

| Type | Primary purpose |
|------|----------------|
| `copilot-instructions.md` | Cross-cutting rules always in context |
| `.instructions.md` | Persistent rules auto-applied to matching files |
| `.prompt.md` | One-shot task with explicit inputs |
| `.SKILL.md` | Domain knowledge loaded on demand |
| `.agent.md` | Autonomous multi-phase workflow |

When in doubt, prefer the simplest type that satisfies the use case. Escalate (e.g. prompt → agent) only when the simpler type cannot express the required behaviour.

---

## Naming Conventions

- All file names: `kebab-case`.
- Standard paths:

| Type | Path |
|------|------|
| Global rules | `.github/copilot-instructions.md` |
| Instructions | `.github/instructions/<concern>.instructions.md` |
| Language-scoped | `.github/instructions/<lang>/<concern>.instructions.md` |
| Prompts | `.github/prompts/<name>.prompt.md` |
| Skills | `.github/skills/<name>.SKILL.md` |
| Agents | `.github/agents/<name>.agent.md` |

- Use subfolders when scoping by language (e.g., `instructions/c-sharp/`).
- Never place customization files outside `.github/`.

---

## File-Type Rules

### `copilot-instructions.md`

- One file per repo, located at `.github/copilot-instructions.md`.
- Global cross-cutting rules only — do not duplicate rules already in an `.instructions.md` file.
- Organise into named sections (`## Clean Code`, `## Security`, etc.).
- No frontmatter — loaded automatically by VS Code Copilot for every chat.
- Keep total line count low; every line is always in context.

### `.instructions.md`

Frontmatter:

    ---
    applyTo: "<glob>"   # "**" = global | "**/*.cs" | ".github/**/*.md"
    ---

- One concern per file. Split when adding a second concern doubles the file.
- Directives only: `Do X` / `Never Y`. Delete rationale that adds no new information.
- ~50 items max. Split into domain files beyond that.
- `applyTo: "**"` only for cross-cutting rules. Be specific when the rule is language/domain-scoped.

### `.prompt.md`

- One objective per file. State it in the first sentence.
- List required inputs explicitly (inline or in a `## Inputs` section).
- Reference skills/instructions with `See <file>` — never duplicate their content.
- Delegate research to subagents; don't inline retrieval logic.

### `.SKILL.md`

- Domain knowledge only: facts, patterns, canonical examples.
- Declarative statements, not imperatives ("Prefer X" not "You should use X").
- No frontmatter required. If provided, only `name:` and `description:` are used.

### `.agent.md`

Frontmatter fields:

| Field | Required | Description |
|---|---|---|
| `name:` | ✓ | Machine identifier (kebab-case). Used in `agents:` references. |
| `description:` | ✓ | One-line human-readable purpose. Shown in VS Code agent picker. |
| `tools:` | ✓ | Allowed tool categories. Valid values: `read`, `edit`, `execute`, `search`, `todo`, `agent`, `web`, `vscode/askQuestions`. |
| `agents:` | When using `agent` tool | List of agent `name:` values this agent may delegate to. |
| `handoffs:` | Recommended | VS Code sidebar action buttons (see format below). |
| `user-invocable: false` | Orchestrator-only agents | Hides the agent from the user-facing agent picker. |

`handoffs:` entry format:

```yaml
handoffs:
  - label: Button label shown to user
    agent: target-agent-name
    prompt: 'Instruction sent to the target agent on click.'
    send: true   # true = send immediately; false = pre-fill input box
```
- Before listing any tool, confirm it is available in the current VS Code Copilot version (`tool_search_tool_regex` or the agent picker).
- Remove tools that are no longer available; never assume a tool still exists across updates.
- Request only the minimum set of tools the agent actually needs.
- MCP server tools can also be listed using `<server>/<tool>` or `<server>/*` notation (e.g. `microsoftdocs/mcp/*`).
- Reference `copilot-instructions.md` and relevant `instructions/` files; **never duplicate** their rules.
- Structure as phases: **Orient → Understand → Analyse → Output**.
- Output section must specify the exact format (table columns, markdown structure, code block type).
- When required inputs are missing, ask rather than infer silently.

---

## Quality Gates

Before saving any Copilot file, verify:

| Check | Pass condition |
|---|---|
| Single responsibility | One clear purpose; no mixed concerns |
| No duplication | Nothing already covered by a loaded file |
| Frontmatter valid | YAML parses; `applyTo` / `tools` use supported values |
| Paths verified | Every referenced path exists in the workspace |
| No padding | No intro/outro sentences; no filler phrases |
| Shortest form | Content cannot be cut further without losing meaning |
| Tool names valid | Every tool in `.agent.md` verified via `tool_search_tool_regex` or the agent picker |
| `applyTo` not over-broad | `**` used only when the rule is genuinely cross-cutting |
| Agent phases complete | All four phases present: Orient → Understand → Analyse → Output |
| `agents:` declared | Every subagent invoked via the `agent` tool is listed in `agents:` |
