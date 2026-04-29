---
description: "Generate a structured PR description from changed files. Use before opening a pull request."
argument-hint: "Optional: PR title, linked issue number, or extra context"
agent: agent
model: GPT-4.1 (copilot)
tools: [execute]
---

Generate a pull request description from the current changes. Incorporate any user-provided context (`$args`).

## Pipeline

Execute steps in order. Do not skip.

### 1 — Detect base branch

```pwsh
git remote show origin | Select-String "HEAD branch"
```

Use the result as `<base>`. Default to `main` on failure.

### 2 — Collect diff

```pwsh
git diff <base>...HEAD
```

If empty, stop and tell the user there are no changes.

### 3 — List changed files

```pwsh
git diff --name-status <base>...HEAD
```

### 4 — Generate description

Using the diff and file list, write the PR description per the rules and structure below.

## Writing Rules

- Reference file names, function/class names, concrete behaviour — no vague summaries.
- Every sentence must carry information. Omit empty sections.
- No marketing language ("seamlessly", "robust", "powerful", "enhance").
- Factual tone — describe what the code does.
- Bullet lists in Changes; prose only in Summary and Motivation.
- One idea per bullet.

## Output Structure

### Summary

One or two sentences. State _what_ this PR does, not _how_.

### Motivation

Why was this change needed? Reference bug, issue, requirement, or tech debt. Include `Closes #<n>` if applicable.

### Changes

Bullet list. Group by area (feature, fix, refactor, config) if >5 items.

### Testing

How was this tested? Automated tests added? Manual steps? If untestable, explain why.

### Breaking Changes

List breaking changes and migration steps. **Omit section entirely if none.**

### Reviewer Checklist

- [ ] Code follows project conventions
- [ ] Tests present or justified
- [ ] No secrets, debug artifacts, or hardcoded credentials
- [ ] Docs updated if behaviour changed
