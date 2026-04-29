---
name: clean-code-review
description: "Review code for clean code and readability issues. Use when reviewing for readability, clean code, complexity, DRY violations, magic numbers, dead code, or maintainability at the function level."
---

# Clean Code Review

Evaluate readability and maintainability at the function/method level. Check every item below that applies.

## Checklist

### Complexity
- Cyclomatic complexity too high (deeply nested conditions, many branches)
- Functions/methods exceeding ~20 lines without clear reason
- Arrow code: deeply nested `if`/`for`/`try` blocks (prefer early returns, guard clauses)
- Complex boolean expressions without extraction into named variables/methods

### Clarity (function/variable level)
- Magic numbers or strings without named constants
- Boolean parameters without named arguments or enums
- Unclear variable/parameter names that require reading the body to understand
- Comments explaining "what" instead of "why" (code should explain what)
- Commented-out code left in place

### DRY
- Duplicated logic across functions/methods/classes
- Copy-paste code with minor variations (extract and parameterize)
- Repeated conditional checks that should be a single function

### Dead Code
- Unreachable code after returns/throws
- Unused variables, parameters, imports, or functions
- Feature flags or branches for removed features
- TODO/FIXME comments older than the current task scope

### Consistency
- Mixed patterns for the same concern within the same scope
- Inconsistent error handling strategy across related functions
- Style switches mid-file (naming, formatting, idiom choice)

## Output

For each finding:
- Show the problematic code
- State the readability or maintainability impact
- Provide the cleaner version
