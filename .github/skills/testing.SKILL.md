# Testing

Domain knowledge on testing strategy, patterns, and conventions.

---

## Test pyramid

```
        /\
       /E2E\          few — slow, brittle, expensive
      /──────\
     /Integration\    moderate — test boundaries and wiring
    /──────────────\
   /   Unit Tests   \  many — fast, isolated, deterministic
  /──────────────────\
```

- **Unit tests**: test a single class/function in isolation. No I/O, no database, no network.
- **Integration tests**: test the interaction between components (e.g. repository against a real DB, API endpoint via `WebApplicationFactory`).
- **E2E tests**: test the full system from the outside. Use sparingly; reserve for critical user journeys.

---

## Microsoft test levels (L0 – L4)

Label every test class or file with its level to make execution scope explicit.

| Level | Category | Description | Requires |
|-------|----------|-------------|----------|
| **L0** | Unit | Fast, in-memory unit tests. No external dependencies whatsoever. | Assembly under test only |
| **L1** | Unit | Unit tests that may touch the file system or other in-process resources, but still no network or external services. | Assembly under test only |
| **L2** | Functional | Functional tests that depend on external resources such as a database, file system, or other services — but run locally. | Assembly + local dependencies (e.g. SQL, file system) |
| **L3** | Functional | Functional tests that run against a deployed but testable service instance. Key dependencies may be replaced with stubs. | Service deployment + optional stubs |
| **L4** | Integration | Full integration tests that run against production (or a production-equivalent). Restricted use; must not mutate production data. | Full product deployment |

### Guidelines

- **L0/L1 are the baseline.** The vast majority of tests are L0 or L1 — fast, deterministic, no infrastructure needed.
- **L2 requires a local environment** (e.g. Docker Compose, LocalDB); typically gated in CI behind a dedicated step.
- **L3 requires a deployed environment.** Well-suited for smoke tests and contract validation post-deployment.
- **L4 is restricted** to controlled pipelines only. L4 tests must not delete, mutate, or depend on specific production data.
- Test projects and files are marked with their level via a trait/category attribute:

```csharp
// C# — xUnit trait
[Trait("Level", "L0")]
public class OrderServiceTests { … }
```

```typescript
// TypeScript — describe label convention
describe('[L0] OrderService', () => { … });
```

---

## AAA pattern

Every test follows **Arrange → Act → Assert**:

```csharp
[Fact]
public async Task PlaceOrder_WhenItemsIsEmpty_ThrowsArgumentException()
{
    // Arrange
    var sut = new OrderService(Substitute.For<IOrderRepository>());

    // Act
    var act = () => sut.PlaceOrderAsync(new PlaceOrderCommand(Items: []));

    // Assert
    await act.Should().ThrowAsync<ArgumentException>()
        .WithMessage("*items*");
}
```

Rules:
- One logical assertion per test (multiple `Should()` calls on the same result are fine).
- No logic (`if`, loops) inside test bodies.
- Never share mutable state between tests.

---

## Test naming

Format: `MethodName_Condition_ExpectedOutcome`

| Good | Bad |
|------|-----|
| `PlaceOrder_WhenItemsIsEmpty_ThrowsArgumentException` | `Test1` |
| `GetUser_WithValidId_ReturnsUser` | `GetUserTest` |
| `ProcessPayment_WhenCardDeclined_PublishesPaymentFailedEvent` | `ShouldWork` |

JS/TS format: `'methodName - condition - expected outcome'` inside `describe` blocks.

---

## Test doubles

Use the least-powerful double that satisfies the test:

| Double | Use when |
|--------|----------|
| **Stub** | Need a canned return value; don't care about calls |
| **Spy** | Need to verify a method was called |
| **Mock** | Need strict call verification with ordered expectations |
| **Fake** | Need a working in-memory implementation (e.g. `InMemoryRepository`) |

C# preferred libraries: `NSubstitute` (stubs/spies/mocks), `FluentAssertions` (assertions).
JS/TS: `vi.fn()` / `jest.fn()` for mocks, `vi.spyOn()` for partial mocking.

Avoid mocking what you don't own (third-party libraries). Wrap them in an adapter and mock the adapter.

---

## What to test vs. what not to test

**Test:**
- Observable behaviour (public API, return values, thrown exceptions, published events).
- Business rules and invariants.
- Edge cases and boundary values.
- Error paths and exception handling.

**Do not test:**
- Private methods directly (test them through the public API).
- Implementation details (which private field was set).
- Framework code (ASP.NET Core routing, EF Core queries — use integration tests for boundaries).
- Trivial getters/setters with no logic.

---

## Test determinism

Tests must be deterministic — same input, same output, always:

- No `DateTime.Now` / `DateTime.UtcNow` in production code — inject `IDateTimeProvider` and stub it.
- No `Random` without a seeded instance — inject randomness.
- No `Thread.Sleep` / `Task.Delay` — use virtual time or awaitable fakes.
- No dependency on file system paths, environment variables, or external services without a test double.

---

## Coverage philosophy

- **Coverage is a floor, not a ceiling.** 80 % line coverage means nothing if the 20 % contains the critical paths.
- Prioritise coverage of: domain logic, edge cases, error handling, security boundaries.
- A test that is hard to write signals a design problem — fix the design, not the test.
- Delete tests that assert implementation details; they become a maintenance burden.

---

## Integration tests with ASP.NET Core

```csharp
public class OrdersApiTests(WebApplicationFactory<Program> factory)
    : IClassFixture<WebApplicationFactory<Program>>
{
    [Fact]
    public async Task PostOrder_WithValidPayload_Returns201()
    {
        var client = factory.CreateClient();

        var response = await client.PostAsJsonAsync("/orders", new { Items = new[] { "item-1" } });

        response.StatusCode.Should().Be(HttpStatusCode.Created);
    }
}
```

Use `WebApplicationFactory<TProgram>` to test the full ASP.NET Core pipeline.
Replace infrastructure dependencies with fakes via `factory.WithWebHostBuilder(...)`.
