---
mode: agent
description: Structured code review on specific files or vs a base branch (default: origin/main). Supports C#, JavaScript, and TypeScript.
tools:
  - run_in_terminal
  - read_file
  - semantic_search
  - grep_search
---

# Code Review

Apply the severity model and language rules from the corresponding instruction files.

## Step 1 – Detect mode

- User specifies a file → **Mode A**: `read_file` the full file, then review.
- "Review my changes" or no file → **Mode B**: diff against `origin/main` (or `$BASE`).

## Step 2 – Gather scope

**Mode B:**
1. `git diff $BASE...HEAD -- "*.cs"` (C#) and/or `-- "*.ts" "*.tsx" "*.js" "*.jsx"` (JS/TS).
2. For each changed hunk, `read_file` the **full enclosing function/method body** (not just changed lines).
3. Also flag unchanged lines that interact with the change.

## Step 3 – Review and output

Evaluate Quality, Performance, and Security for every file/hunk. Apply language-specific rules. Produce output with the format from `code-review.instructions.md`.

## Completion criteria

- [ ] All in-scope files reviewed.
- [ ] Every finding has a line reference, impact statement, and suggested fix.
- [ ] Summary table present.
