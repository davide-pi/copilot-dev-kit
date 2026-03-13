---
mode: agent
description: Generate unit and integration tests for a given file or function, with coverage rationale and edge cases.
tools:
  - read_file
  - semantic_search
  - grep_search
---

# Test Writer

## Step 1 – Read and detect

`read_file` the target. If not specified, ask. Detect framework: search `*.csproj` for `xunit`/`nunit`/`mstest`, or `package.json` for `jest`/`vitest`/`@testing-library`.

## Step 2 – Coverage rationale

List one line per case before writing any code: **happy path**, **edge cases** (boundary values, empty, null), **error cases** (exceptions, rejected promises, invalid input), **behaviour contracts**.

## Step 3 – Generate tests (AAA, one assertion per test)

Name tests: `MethodName_Condition_ExpectedOutcome` (C#) or `'MethodName - condition - expected outcome'` (JS/TS).

### C# (xUnit / NUnit)

```csharp
[Fact]
public void PlaceOrder_WhenItemsIsEmpty_ThrowsArgumentException()
{
    // Arrange
    var sut = new OrderService();

    // Act
    var act = () => sut.PlaceOrder([]);

    // Assert
    act.Should().Throw<ArgumentException>();
}
```

- Use `FluentAssertions` + `NSubstitute` when available. Async tests: `async Task` return type.

### JavaScript / TypeScript (Jest / Vitest)

```typescript
describe('placeOrder', () => {
  it('throws when items array is empty', () => {
    const sut = new OrderService();
    expect(() => sut.placeOrder([])).toThrow('items cannot be empty');
  });
});
```

- React components: `@testing-library/react`; query by role/label/text only.

## Step 4 – Output

Produce the complete test file, ready to compile and run. Add a brief comment above each test group.
