---
mode: agent
description: Generate or update documentation for existing code — XML docs (C#), JSDoc/TSDoc (JS/TS), and module-level README.
tools:
  - read_file
  - grep_search
---

# Documentation Generator

## Step 1 – Read the target

`read_file` the file or module the user wants documented.
If the user did not specify a target, ask before proceeding.

## Step 2 – Detect language and doc format

| File extension | Documentation format |
|----------------|----------------------|
| `.cs` | XML documentation comments (`/// <summary>`) |
| `.ts` / `.tsx` | TSDoc (`/** @param … @returns … */`) |
| `.js` / `.jsx` | JSDoc (`/** @param {Type} … @returns {Type} … */`) |

## Step 3 – Generate documentation

### C# — XML docs

Add `/// <summary>` to every `public` and `internal` type member that lacks one.

```csharp
/// <summary>
/// Processes the order and returns a confirmation identifier.
/// </summary>
/// <param name="order">The order to process. Must not be null.</param>
/// <param name="ct">Cancellation token.</param>
/// <returns>The unique confirmation ID assigned to the order.</returns>
/// <exception cref="ArgumentNullException">Thrown when <paramref name="order"/> is null.</exception>
public Task<Guid> ProcessOrderAsync(Order order, CancellationToken ct = default);
```

Rules:
- `<summary>`: one sentence, imperative mood, no period.
- `<param>`: describe the purpose and any constraints (null, range).
- `<returns>`: describe what the value represents, not its type.
- `<exception>`: document every exception that can escape the method.
- Do not document auto-properties with trivial names (`Id`, `Name`) unless the meaning is non-obvious.

### TypeScript — TSDoc

```typescript
/**
 * Processes the order and returns a confirmation identifier.
 *
 * @param order - The order to process.
 * @param options - Optional processing configuration.
 * @returns The unique confirmation ID assigned to the order.
 * @throws {ArgumentError} When the order items list is empty.
 */
async function processOrder(order: Order, options?: ProcessOptions): Promise<string>
```

### JavaScript — JSDoc

```javascript
/**
 * Processes the order and returns a confirmation identifier.
 *
 * @param {Order} order - The order to process.
 * @param {ProcessOptions} [options] - Optional processing configuration.
 * @returns {Promise<string>} The unique confirmation ID assigned to the order.
 */
async function processOrder(order, options) {}
```

## Step 4 – Module README (optional)

If requested, generate a `README.md` with: description, exports table (`Symbol | Kind | Description`), usage example, and dependencies list.

## Rules

- Document only what is not already obvious from the name and type signature.
- Do not document private implementation details.
- Do not alter any logic — doc-only changes.
