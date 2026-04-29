---
applyTo: "**/*.cs"
---

# C# / ASP.NET Core Conventions

## Naming

- `PascalCase`: classes, methods, properties, events, constants, `static readonly`.
- `camelCase`: locals, parameters.
- `_camelCase`: private fields.
- Prefix interfaces with `I` (`IOrderRepository`).
- Suffix async methods with `Async` (`GetOrderAsync`).

## Async / Await

- Always `await` tasks — never `.Result` or `.Wait()`.
- Propagate `CancellationToken` through the entire call chain.
- Return `Task`/`Task<T>`; avoid `async void` except event handlers.
- `ConfigureAwait(false)` in library code only.

## Nullable Reference Types

- `<Nullable>enable</Nullable>` in every project.
- Never suppress with `!` unless verified non-null; comment why.
- Initialise all non-nullable reference properties in the constructor or via `required init`.

## Records and Immutability

- Prefer `record`/`record struct` for DTOs, value objects, domain events.
- Use `init`-only setters for write-once properties.
- Model state changes as new instances — avoid mutable domain objects.

## Pattern Matching

- `switch` expressions over `if/else` chains for exhaustive dispatch.
- `is` patterns for null checks and type tests.
- List patterns (`[var first, ..]`) for sequence processing.

## Dependency Injection

- Register via extension methods on `IServiceCollection` — no `ServiceLocator`.
- Constructor injection only; no property injection.
- Lifetime: Transient (stateless), Scoped (per-request), Singleton (thread-safe shared).
- Never inject `IServiceProvider` into business logic.

## ASP.NET Core API

- Pick minimal APIs or controllers per project — do not mix.
- Return `IActionResult`/`Results<T>` — never raw objects.
- Validate at the API boundary (Data Annotations or FluentValidation); never inside domain logic.
- `[ProducesResponseType]` on every action.
- `ProblemDetails` (RFC 9457) for error responses.

## Error Handling

- Custom exceptions deriving from a base `DomainException` for domain errors.
- Handle infrastructure exceptions at the boundary (controller/middleware) — never swallow silently.
- Never catch bare `Exception` in domain or application logic.

## LINQ

- Method syntax over query syntax.
- Materialise with `.ToList()`/`.ToArray()` to avoid multiple enumeration.
- No LINQ inside tight loops — prefer a single query.

## Code Style

- `var` when the type is obvious from the RHS; explicit types otherwise.
- Expression-bodied members for single-expression properties and methods.
- Primary constructors (C# 12+) where they reduce boilerplate.
- One type per file; file name = type name.
