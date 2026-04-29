---
name: ubiquitous-language
description: Extract a DDD-style ubiquitous language glossary from the current conversation, flagging ambiguities and proposing canonical terms. Saves to UBIQUITOUS_LANGUAGE.md. Use when user wants to define domain terms, build a glossary, harden terminology, create a ubiquitous language, or mentions "domain model" or "DDD".
---

# Ubiquitous Language

Extract and formalize domain terminology from the current conversation into a glossary saved to `UBIQUITOUS_LANGUAGE.md`.

## Process

1. **Scan** the conversation for domain nouns, verbs, and concepts.
2. **Identify problems**: ambiguity (same word, different concepts), synonyms (different words, same concept), vague/overloaded terms.
3. **Propose canonical glossary** with opinionated term choices.
4. **Write to `UBIQUITOUS_LANGUAGE.md`** using the format below.
5. **Output a summary** inline.

## Output Format

```md
# Ubiquitous Language

## {Group name}

| Term | Definition | Aliases to avoid |
| --- | --- | --- |
| **Term** | One-sentence definition | alias1, alias2 |

## Relationships

- A **Term** belongs to exactly one **OtherTerm**

## Example dialogue

> **Dev:** "Question using terms precisely?"
> **Domain expert:** "Answer clarifying boundaries."

## Flagged ambiguities

- "word" was used to mean both **X** and **Y** — these are distinct: ...
```

## Rules

- **Be opinionated.** Pick the best term; list alternatives as aliases to avoid.
- **Flag conflicts.** Ambiguous terms go in "Flagged ambiguities" with a clear recommendation.
- **Domain terms only.** Skip generic programming concepts unless they have domain-specific meaning.
- **One-sentence definitions.** Define what it IS, not what it does.
- **Show relationships.** Bold term names, express cardinality where obvious.
- **Group naturally.** Multiple tables when clusters emerge (by subdomain, lifecycle, actor). One table is fine if all terms are cohesive — don't force groupings.
- **Example dialogue.** 3–5 exchanges between dev and domain expert demonstrating how terms interact and clarifying boundaries between related concepts.

## Re-running

When invoked again in the same conversation:
1. Read existing `UBIQUITOUS_LANGUAGE.md`
2. Incorporate new terms
3. Update evolved definitions
4. Re-flag new ambiguities
5. Rewrite example dialogue to include new terms
