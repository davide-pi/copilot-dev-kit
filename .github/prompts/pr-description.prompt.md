---
mode: agent
description: Generate a structured Pull Request description from the current branch diff.
tools:
  - run_in_terminal
---

# PR Description Generator

## Step 1 – Gather diff

Run `git diff origin/main...HEAD --stat` to list changed files, then `git diff origin/main...HEAD` for the full diff.
If the base branch is not `origin/main`, use what the user specified (`$BASE`).

## Step 2 – Generate the PR description

Produce a PR description using **exactly** this structure:

```markdown
## Context

<!-- What problem does this PR solve? What is the business or technical motivation? -->

## Changes

<!-- Bullet list of what was changed and why, grouped by concern. -->
<!-- One bullet per logical change. Be specific: avoid "misc fixes". -->

## Breaking changes

<!-- List any changes to public API surfaces, contracts, serialization formats, or events.  -->
<!-- If none: "None." -->

## How to test

<!-- Steps a reviewer can follow to verify the change works correctly. -->
<!-- Include: setup, commands to run, expected outcome. -->

## Checklist

- [ ] Tests added / updated
- [ ] Documentation updated
- [ ] No secrets or sensitive data introduced
- [ ] Breaking changes documented above
```

## Rules

- Base the description strictly on the diff — do not invent changes.
- Keep each section concise; prefer bullet points over prose.
- If a section is empty (e.g. no breaking changes), write `None.` — never omit the section.
- Do not include reviewer assignments, labels, or milestone suggestions.
