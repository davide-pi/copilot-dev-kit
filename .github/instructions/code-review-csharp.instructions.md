---
applyTo: "**/*.cs"
---

# Code Review – C# / ASP.NET Core

Apply the conventions from `csharp.instructions.md` with the severity model from `code-review.instructions.md`.

## Severity assignments

**Naming & Style**
- Fields not `_camelCase`, constants not `PascalCase`, async methods missing `Async` suffix → **[IMPORTANT]**
- Wrong modifier order, missing braces on `if`/`else`/`for`/`foreach`, `using` inside namespace → **[IMPORTANT]**
- `var` when type is not obvious, System imports not sorted first → **[SUGGESTION]**

**Async / Await**
- `.Result`/`.Wait()` inside a request pipeline → **[CRITICAL]**; elsewhere → **[IMPORTANT]**
- Missing `CancellationToken` on public I/O methods, `async void` outside event handlers → **[IMPORTANT]**

**Dependency Injection**
- Scoped service injected into singleton (captive dependency) → **[CRITICAL]**
- `IConfiguration` in domain/application services, `ServiceLocator`/`IServiceProvider` in business logic → **[IMPORTANT]**

**Security**
- Raw SQL string interpolation → **[CRITICAL]**
- Sensitive data (tokens, passwords, PII) logged → **[CRITICAL]**
- Missing `[Authorize]` on non-public controller/action → **[CRITICAL]**
- CORS `AllowAll` without justification → **[IMPORTANT]**

**Error Handling**
- Empty `catch`, bare `catch (Exception)` → **[IMPORTANT]**
- String interpolation in log calls (use structured logging) → **[IMPORTANT]**

**Nullable**
- `!` suppression without explanatory comment → **[IMPORTANT]**

**Readonly & Immutability**
- Constructor-only fields not marked `readonly` → **[SUGGESTION]**

**LINQ & Performance**
- Multiple enumeration of `IEnumerable<T>`, N+1 query patterns → **[IMPORTANT]**

Use `csharp` code blocks in all findings.
