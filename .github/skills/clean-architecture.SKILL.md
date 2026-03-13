# Clean Architecture

Domain knowledge on Clean Architecture applied to ASP.NET Core projects.

---

## Core principle: the Dependency Rule

Dependencies point **inward only**:

```
Presentation  ──►  Application  ──►  Domain
Infrastructure ──►  Application  ──►  Domain
```

- **Domain** has zero dependencies on frameworks, ORMs, or external systems.
- **Application** depends only on Domain. It orchestrates use cases and defines interfaces.
- **Infrastructure** implements the interfaces defined in Application/Domain (repositories, email, storage, etc.).
- **Presentation** (API controllers, minimal API endpoints) depends on Application via command/query objects.

---

## Layer responsibilities

### Domain

- Entities: objects with identity and lifecycle (`Order`, `Customer`).
- Value Objects: immutable, equality by value (`Money`, `Address`). Prefer `record`.
- Domain Events: signal something meaningful happened (`OrderPlaced`, `PaymentFailed`).
- Domain Services: stateless logic that doesn't belong to a single entity.
- Interfaces: `IRepository<T>`, `IDomainEventDispatcher` — defined here, implemented in Infrastructure.
- No `DbContext`, no `HttpClient`, no framework attributes.

### Application

- Use Cases / Commands / Queries: one class per use case (`PlaceOrderCommand`, `GetOrderByIdQuery`).
- Handlers: implement `ICommandHandler<T>` / `IQueryHandler<T,R>`.
- Application Services: orchestrate multiple domain operations; own the transaction boundary.
- DTOs: data shapes crossing the Application boundary (never expose Domain entities directly).
- Validation: input validation lives here (FluentValidation, DataAnnotations) — not inside Domain.
- No UI concerns, no HTTP, no SQL.

### Infrastructure

- Repository implementations (`EfOrderRepository : IOrderRepository`).
- `DbContext` and EF Core configurations.
- External service clients (email, SMS, payment gateway, blob storage).
- Background jobs, message bus publishers/consumers.
- `appsettings.json` binding and options classes.

### Presentation

- ASP.NET Core controllers or minimal API endpoints.
- Request/response models (separate from Application DTOs).
- Mapping between HTTP models and Application commands/queries.
- Middleware, filters, authentication/authorization setup.

---

## Ports & Adapters (Hexagonal Architecture)

Clean Architecture is a flavour of Ports & Adapters:

- **Port** = interface defined in Application/Domain (`IOrderRepository`, `IEmailSender`).
- **Adapter** = Infrastructure implementation (`EfOrderRepository`, `SendGridEmailSender`).
- Adapters can be swapped without touching Domain or Application logic (e.g. swap EF Core for Dapper).

---

## Common violations to avoid

| Violation | Problem |
|-----------|---------|
| `DbContext` injected into Domain or Application layer | Infrastructure leak |
| `HttpContext` accessed inside a Domain Service | Presentation leak |
| Business logic placed in a controller action | Inverted dependency |
| Domain entity returned directly from API endpoint | Couples transport to domain model |
| `IConfiguration` injected into a Domain Service | Infrastructure leak |
| Static `ServiceLocator` / `IServiceProvider` inside business logic | Hidden dependency |

---

## Project structure (recommended)

```
src/
  MyApp.Domain/           # Entities, VOs, Domain Events, Interfaces
  MyApp.Application/      # Use cases, Handlers, DTOs, Validators
  MyApp.Infrastructure/   # EF Core, repositories, external services
  MyApp.Api/              # ASP.NET Core controllers/endpoints, DI wiring
tests/
  MyApp.Domain.Tests/
  MyApp.Application.Tests/
  MyApp.Infrastructure.Tests/
```

---

## Dependency injection wiring

Wire everything in the Composition Root (`Program.cs` / `Startup.cs`):

```csharp
// Infrastructure registers its own implementations
builder.Services.AddScoped<IOrderRepository, EfOrderRepository>();

// Application registers handlers
builder.Services.AddMediatR(cfg => cfg.RegisterServicesFromAssembly(typeof(PlaceOrderCommand).Assembly));
```

Never call `new` on an infrastructure dependency inside Application or Domain.
