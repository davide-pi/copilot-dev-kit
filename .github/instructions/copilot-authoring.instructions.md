---
applyTo: ".github/**/*.md"
---

# Copilot File Authoring

Rules for creating and editing `.instructions.md`, `.prompt.md`, `.SKILL.md`, and `.agent.md` files.

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

## File-Type Rules

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
- Keep reference tables short. Link authoritative sources when available.
- Skills are loaded on demand — every unnecessary line costs tokens at invocation.

### `.agent.md`

Frontmatter:

    ---
    name: <kebab-case>
    description: <one sentence, ≤120 chars — used in agent picker>
    tools:
      - read | search | execute | agent | todo | vscode/askQuestions  # list only what is needed
    ---

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
