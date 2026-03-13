---
mode: ask
description: Generate a Conventional Commits message for staged or specified changes.
tools:
  - run_in_terminal
---

# Commit Message Generator

## Step 1 – Gather changes

Run `git diff --cached` to inspect staged changes.
If nothing is staged, run `git diff HEAD` and note that the user should stage before committing.

## Step 2 – Generate the commit message

Follow the **Conventional Commits** specification (https://www.conventionalcommits.org):

```
<type>(<scope>): <short summary>

[optional body]

[optional footer(s)]
```

### Type — choose the most specific that applies

| Type | When to use |
|------|-------------|
| `feat` | New feature visible to users or consumers |
| `fix` | Bug fix |
| `refactor` | Code change that neither fixes a bug nor adds a feature |
| `perf` | Performance improvement |
| `test` | Adding or updating tests |
| `docs` | Documentation only |
| `build` | Build system, dependencies, CI |
| `chore` | Maintenance tasks not affecting production code |
| `revert` | Reverting a previous commit |

### Rules

- **Subject line**: imperative mood, lowercase after the colon, no period, ≤ 72 characters.
  - Good: `feat(auth): add refresh token rotation`
  - Bad: `Added refresh token rotation.`
- **Scope**: optional, lowercase, single word or kebab-case — matches the module, file, or domain area changed.
- **Body**: explain *what* and *why*, not *how*. Wrap at 72 characters. Add a blank line before the body.
- **Breaking change footer**: mandatory when the change breaks a public interface.
  ```
  BREAKING CHANGE: <description of what changed and migration path>
  ```
- **Issue reference footer**: `Closes #123` or `Refs #456` on its own line.

## Step 3 – Output

Produce only the commit message — no explanations, no markdown fences around it.
If multiple logical changes are staged, suggest splitting into separate commits and provide a message for each.
