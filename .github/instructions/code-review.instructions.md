---
applyTo: "**"
---

# Code Review

## Severity

| Label | Criteria | Gate |
|---|---|---|
| 🔴 **[CRITICAL]** | OWASP Top 10, data loss, broken auth, unhandled crash | Must fix before merge |
| 🟡 **[IMPORTANT]** | Logic bugs, contract violations, missing error handling, swallowed exceptions, magic strings | Should fix before merge |
| 🟢 **[SUGGESTION]** | Style, readability, naming, minor improvements | Nice to have |

## Output Rules

- Link every finding: `[Symbol](path/to/file#L42)` + 1–3 line code quote.
- Include Before/After snippet for every finding.
- Use "Consider / Prefer / Replace X with Y" framing.
- Evaluate every finding across Quality, Performance, Security.
- Order: CRITICAL → IMPORTANT → SUGGESTION. Omit empty sections.
- Never invent findings.

## Finding format

**{🔴/🟡/🟢} {SEVERITY} – {Category}: {title}** · [{Symbol}:{line}](path#L{line})

> 1–3 line code quote

Problem description.

**Why this matters:** consequence.

**Suggested fix:** Before/After snippet. **Reference:** (omit if not applicable)

## Review structure

Sections: `### 🔴 CRITICAL`, `### 🟡 IMPORTANT`, `### 🟢 SUGGESTION` (omit empty). End with:

**Summary:** 🔴 N critical · 🟡 N important · 🟢 N suggestions

| # | Severity | Symbol | Issue |
|---|---|---|---|

Summary table is always required.
