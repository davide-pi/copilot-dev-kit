# Copilot Instructions

These rules apply to every file in this repository, regardless of language or framework.

---

## Clean Code

- Names must reveal intent: variables, functions, and classes should not require a comment to explain what they do.
- Functions do one thing. If a function needs a section comment, extract a new function.
- Keep functions short: aim for ≤ 20 lines; never exceed 50 without a strong reason.
- Avoid double negatives and boolean traps (`!isNotValid`, `flag2`).
- Prefer positive conditions (`if (isValid)` over `if (!isInvalid)`).
- Delete commented-out code. Version control is the history.
- No magic numbers or magic strings — use named constants.

---

## SOLID

- **S** — Single Responsibility: one class, one reason to change.
- **O** — Open/Closed: extend behaviour through new code, not by modifying existing code.
- **L** — Liskov Substitution: subtypes must be usable in place of their base type without breaking behaviour.
- **I** — Interface Segregation: prefer narrow, focused interfaces over fat ones.
- **D** — Dependency Inversion: depend on abstractions, not concretions. Inject dependencies, never instantiate them inside business logic.

---

## DRY / KISS / YAGNI

- **DRY**: every piece of knowledge has a single, authoritative representation. Duplication is a bug waiting to happen.
- **KISS**: the simplest solution that satisfies the requirement is correct. Complexity must be justified.
- **YAGNI**: do not implement behaviour that is not required today. Speculative abstractions become maintenance debt.

---

## Clean Architecture

- Dependencies point inward: Domain ← Application ← Infrastructure / Presentation.
- Domain has zero dependencies on frameworks, ORMs, or external systems.
- Application layer orchestrates use cases; it must not contain infrastructure concerns.
- Cross-cutting concerns (logging, caching, validation) live at the boundary, not inside domain logic.
- Interfaces are defined where they are needed (Domain / Application), implemented where they belong (Infrastructure).

---

## Testing mindset

- Every non-trivial behaviour must be covered by at least one automated test.
- Tests follow **AAA**: Arrange, Act, Assert — one logical assertion per test.
- Test names describe the scenario: `MethodName_Condition_ExpectedOutcome`.
- Tests must be deterministic: no random data, no sleep, no dependency on external systems without a test double.
- Prefer unit tests at the base of the pyramid; add integration tests at boundaries; use E2E tests sparingly.
- A test that is hard to write signals a design problem — fix the design, not the test.

---

## Security mindset

- Never trust input: validate and sanitise at every system boundary (HTTP, CLI, file, queue).
- Never log sensitive data (passwords, tokens, PII, connection strings).
- Prefer allowlists over denylists for validation.
- Secrets must never be hardcoded — use environment variables or a secrets manager.
- Apply the principle of least privilege: request only the permissions actually needed.
- Treat third-party dependencies as untrusted: pin versions, audit regularly.
- Refer to the OWASP Top 10 as the baseline for web security threats.

---

## Clarification

- Ask before acting when a request is ambiguous and different interpretations would lead to meaningfully different solutions.
- Multiple questions are fine when there are several genuine doubts.
- Only ask if the answer would actually change the outcome; otherwise state the assumption and proceed.
- For every question asked, pre-populate the most likely and sensible answer(s) as a suggestion, so the user can confirm or override rather than answer from scratch.
