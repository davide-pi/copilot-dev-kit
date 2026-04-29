---
applyTo: '**'
description: "Token consumption rules: less is best until context stays clear."
---

# Token Consumption

**Constraint**: minimise total tokens (input + output) per turn. Never sacrifice correctness or clarity.

## Response Shape

- Lead with the answer; explain only if it adds value.
- One sentence > one paragraph > one page.
- Never echo the user's question or restate known context.
- Omit meta-commentary ("Let me…", "I'll now…", "First I need to…").
- Show only changed regions with minimal surrounding context — never entire files.

## Tool Efficiency

- **Batch reads**: one wide `read_file` call over many small ones.
- **Parallel calls**: fire independent calls together; never serialise parallelisable work.
- **Stop early**: act once you have enough context. No redundant confirmation queries.
- **Grep before read**: locate exact lines with `grep_search`, then `read_file` only that range.
- **Subagents for exploration**: delegate broad searches to `Explore` instead of chaining calls in the main thread.

## Context Loading

- Never read an entire file for a single function or section.
- Never re-read files already in conversation context.
- Read multiple files in parallel.
- Prefer `grep_search` with regex alternation (`a|b|c`) over separate searches.

## Output Trimming

- Use `// ...existing code...` for unchanged regions in explanations (never in edit tool calls).
- Omit files that were not changed.
- Filter/truncate terminal output — show only relevant lines.
- Do not repeat information the user can already see.

## Reasoning Discipline

- Internalise intermediate reasoning. Do not narrate decisions unless the user asks.
- State conclusions directly. Show derivation only when the user must verify it.
- Short answers go inline — no file, code block, or list for a one-liner.
