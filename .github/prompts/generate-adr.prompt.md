---
description: "Generate an Architecture Decision Record from the current discussion or context. Use when making or documenting a design decision."
argument-hint: "Required: the decision topic, e.g. 'use PostgreSQL over MongoDB for user data'"
agent: agent
model: GPT-4.1 (copilot)
tools: [read, search, edit, vscode/askQuestions]
---

Generate an Architecture Decision Record (ADR) based on the current conversation context and any user-provided arguments (`$args`).

If the conversation contains a design discussion, extract the decision from it. If `$args` provides the decision topic directly, use that.

## Process

1. **Identify the decision** — What architectural choice was made or needs to be made?
2. **Gather context** — Scan conversation and codebase for constraints, requirements, alternatives discussed.
3. **Write the ADR** — Using the structure below.

## Writing Rules

- Factual, neutral tone — describe trade-offs honestly.
- Be specific: name technologies, patterns, files, and modules involved.
- Every section must carry information. Omit sections that don't apply.
- Alternatives must include genuine options that were considered, not strawmen.
- Consequences must cover both positive and negative outcomes.

## Output Structure

```markdown
# ADR-{NNN}: {Decision Title}

**Status:** Proposed | Accepted | Deprecated | Superseded by ADR-{NNN}
**Date:** {YYYY-MM-DD}
**Deciders:** {who was involved}

## Context

What forces are at play? What problem needs solving? What constraints exist?

## Decision

State the decision clearly in one or two sentences.

## Alternatives Considered

### {Alternative 1}
- **Pros:** ...
- **Cons:** ...
- **Why rejected:** ...

### {Alternative 2}
- **Pros:** ...
- **Cons:** ...
- **Why rejected:** ...

## Consequences

### Positive
- ...

### Negative
- ...

### Risks
- ...

## References

- Links to relevant docs, issues, RFCs, or prior ADRs.
```

## File Placement

Save as `docs/adr/ADR-{NNN}-{kebab-case-title}.md`. Create the directory if it doesn't exist. Determine `{NNN}` by scanning existing ADR files and incrementing. Start at `001` if none exist.
