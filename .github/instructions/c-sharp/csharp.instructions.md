---
applyTo: "**/*.cs"
---

# C# / ASP.NET Core Conventions

These rules apply to all C# files in addition to the language-agnostic rules in `copilot-instructions.md`.

---

## Naming

- Classes, methods, properties, and events: `PascalCase`.
- Local variables and parameters: `camelCase`.
- Private fields: `_camelCase` with leading underscore.
- Constants and static readonly fields: `PascalCase`.
- Interfaces: prefix with `I` — `IOrderRepository`, not `OrderRepository`.
- Async methods: suffix with `Async` — `GetOrderAsync`, not `GetOrder`.
- Generic type parameters: `T`, or descriptive `TEntity`, `TResult`.

---

## Async / Await

- Always `await` Tasks; never `.Result` or `.Wait()`.
- Always propagate `CancellationToken` through the call chain; never ignore it.
- Return `Task` / `Task<T>` from async methods; avoid `async void` except for event handlers.
- Use `ConfigureAwait(false)` in library code; omit it in ASP.NET Core application code.

---

## Nullable Reference Types

- Enable `<Nullable>enable</Nullable>` in every project.
- Never suppress nullable warnings with `!` unless you have verified the value is non-null; add a comment explaining why.
- Use `string?` for genuinely optional strings; use `string` when a value is always required.
- Initialise all non-nullable reference type properties in the constructor or via required init.

---

## Records and Immutability

- Prefer `record` or `record struct` for DTOs, value objects, and domain events.
- Use `init`-only setters for properties that must be set once.
- Avoid mutability in domain objects; model state changes as new instances.

---

## Pattern Matching

- Prefer `switch` expressions over `if/else` chains for exhaustive type or value dispatch.
- Use `is` patterns for null checks and type tests: `if (result is not null)`.
- Use list patterns (`[var first, ..]`) when processing sequences.

---

## Dependency Injection

- Register services via extension methods on `IServiceCollection`; never use `ServiceLocator`.
- Prefer constructor injection; avoid property injection.
- Match service lifetime to usage: Transient for stateless, Scoped for per-request, Singleton for thread-safe shared state.
- Never inject `IServiceProvider` directly into business logic.

---

## ASP.NET Core API

- Use minimal APIs or controllers — be consistent within a project; do not mix.
- Return `IActionResult` / `Results<T>` typed results; never return raw objects from controllers.
- Validate input with Data Annotations or FluentValidation at the API boundary; never inside domain logic.
- Use `[ProducesResponseType]` on every controller action to document all response codes.
- Use `ProblemDetails` format for error responses (RFC 9457).

---

## Error Handling

- Use custom exception types that derive from a base `DomainException` for domain errors.
- Handle infrastructure exceptions at the boundary (controller / middleware); never swallow them silently.
- Never catch `Exception` broadly inside domain or application logic.

---

## LINQ

- Prefer method syntax over query syntax.
- Avoid multiple enumeration of `IEnumerable<T>`; materialise with `.ToList()` or `.ToArray()` when needed.
- Do not use LINQ inside tight loops; prefer a single query.

---

## Code Style

- Use `var` when the type is obvious from the right-hand side; use explicit types otherwise.
- Prefer expression-bodied members for single-expression properties and methods.
- Use primary constructors (C# 12+) for simple classes where it reduces boilerplate.
- One type per file; file name matches type name.
