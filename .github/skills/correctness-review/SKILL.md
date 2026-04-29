---
name: correctness-review
description: "Review code for correctness and logic issues. Use when reviewing for bugs, edge cases, error handling, race conditions, or off-by-one errors."
---

# Correctness Review

Find logic errors and missing edge cases. Check every item below that applies.

## Checklist

### Logic Errors
- Off-by-one in loops, ranges, or boundary conditions
- Wrong comparison operator (`<` vs `<=`, `==` vs `===`)
- Inverted boolean logic or negation errors
- Missing `break`/`return` causing fall-through
- Integer overflow/underflow in arithmetic

### Null & Empty Handling
- Null dereference on paths that can produce null
- Empty collection assumed to have elements
- Default values that silently hide errors
- Missing null propagation in chains

### Error Handling
- Swallowed exceptions (empty catch blocks)
- Catch-all that hides specific failures
- Error state not propagated to caller
- Resource cleanup skipped on error paths (missing finally/using/defer)
- Error messages that leak internal state

### Race Conditions
- Check-then-act without atomicity
- Shared mutable state accessed from multiple threads/requests
- Time-of-check-to-time-of-use (TOCTOU) vulnerabilities
- Missing locks on read-modify-write sequences
- Event ordering assumptions that aren't guaranteed

### Edge Cases
- Empty input, single element, maximum size
- Unicode, special characters, whitespace-only strings
- Timezone and DST transitions in date handling
- Negative numbers, zero, MAX_VALUE boundaries
- Concurrent modification during iteration

### State Management
- State mutation without validation of preconditions
- Inconsistent state after partial failure (no rollback)
- Stale cache or reference after mutation
- Missing state machine transitions or invalid state paths

## Output

For each finding:
- Describe the bug or missing edge case
- Show a concrete scenario that triggers the failure
- Explain the impact (crash, data corruption, wrong result, silent failure)
- Provide the fix
