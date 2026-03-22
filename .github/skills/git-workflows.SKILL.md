# Git Workflows

Canonical git commands for investigation, diffing, history traversal, and bisect workflows.

---

## File History

```bash
# Full history with patch for a specific file (follows renames)
git log --follow -p <file>

# Compact history list for a file (including deleted files)
git log --all --oneline -- <file>

# Show author and commit for each line
git blame <file>

# Blame with line range
git blame -L <start>,<end> <file>

# Show commits that touched a specific function
git log -L :<function-name>:<file>
```

---

## Diff Commands

```bash
# Changes between branch and HEAD (stat summary)
git diff origin/main...HEAD --stat

# Full diff between branch and HEAD
git diff origin/main...HEAD

# Diff for specific file types only
git diff origin/main...HEAD -- "*.cs"
git diff origin/main...HEAD -- "*.ts" "*.tsx" "*.js" "*.jsx"

# Diff for a specific file
git diff origin/main...HEAD -- <file>

# Show what changed in a specific commit
git show <commit-hash>

# Show only changed file names in a commit
git show --name-only <commit-hash>

# Diff between two commits
git diff <commit-a>..<commit-b>
```

---

## Commit Inspection

```bash
# List commits on current branch not on main
git log origin/main...HEAD --oneline

# List commits with author and date
git log origin/main...HEAD --pretty=format:"%h %ad %an | %s" --date=short

# Search commit messages for a keyword
git log --all --oneline --grep="<keyword>"

# Find commits that introduced or removed a specific string
git log -S "<string>" --all --oneline

# Find commits that changed lines matching a regex
git log -G "<regex>" --all --oneline
```

---

## Bisect Workflow

Use when the introducing commit cannot be isolated by reading code alone.

```bash
# Start bisect session
git bisect start

# Mark current state as broken
git bisect bad

# Mark a known good commit
git bisect good <known-good-commit>

# Git checks out the midpoint — test it, then:
git bisect good   # if this commit is fine
git bisect bad    # if this commit is broken

# Git will binary-search until it prints the first bad commit.
# When done, reset:
git bisect reset
```

Bisect is most effective when you can automate the good/bad check:

```bash
git bisect run <test-script>
```

---

## Stash & Working Tree

```bash
# Stash uncommitted changes
git stash push -m "description"

# List stashes
git stash list

# Apply most recent stash
git stash pop

# Show diff of a stash
git stash show -p stash@{0}
```

---

## Remote & Branch Context

```bash
# List all branches (local + remote)
git branch -a

# Show tracking branch for current branch
git branch -vv

# Fetch all remotes without merging
git fetch --all --prune
```
