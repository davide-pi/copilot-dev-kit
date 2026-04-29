---
name: architecture-review
description: "Review code for architecture and design quality. Use when reviewing for design, coupling, SOLID violations, naming, abstraction problems, or structural issues."
---

# Architecture Review

Evaluate the code's structural quality. Check every item below that applies.

## Checklist

### Coupling & Cohesion
- High coupling: concrete dependencies where abstractions belong
- Low cohesion: class/module doing unrelated things
- God class/function: too many responsibilities
- Feature envy: code that manipulates another object's data more than its own
- Inappropriate intimacy: classes that know too much about each other's internals

### Abstraction
- Wrong abstraction level: too concrete (premature detail) or too abstract (over-engineering)
- Leaky abstractions: implementation details exposed through interfaces
- Missing abstraction: duplicated logic that should be extracted
- Premature abstraction: generalizing from a single use case

### SOLID Violations
- **S**: class has multiple reasons to change
- **O**: modification required where extension should suffice
- **L**: subtype breaks parent's contract
- **I**: client forced to depend on methods it doesn't use
- **D**: high-level module depends on low-level implementation

### Naming & Intent (structural level)
- Class/module/interface names that don't describe their responsibility
- Inconsistent naming conventions across the codebase
- Ambiguous names that could mean multiple things at the module boundary

### Boundaries
- Business logic in infrastructure/UI layers
- Infrastructure concerns in domain code
- Missing or misplaced validation
- Cross-cutting concerns handled inconsistently

## Output

For each finding:
- State the design principle violated
- Show the problematic code
- Explain why it will cause problems (maintainability, testability, extensibility)
- Suggest a concrete fix — not a vague "consider refactoring"
