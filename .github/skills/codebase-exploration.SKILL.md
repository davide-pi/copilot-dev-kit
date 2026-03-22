# Codebase Exploration

Domain knowledge for discovering stack, layer topology, test frameworks, naming conventions, and entry points in an unfamiliar codebase.

---

## Stack Detection

Search for manifest files to identify the technology stack. Presence of multiple manifests means a polyglot solution — map each independently.

| Manifest | Stack |
|---|---|
| `*.csproj` | .NET (C#) |
| `*.sln` | .NET solution (may contain multiple projects) |
| `package.json` | Node.js / TypeScript / JavaScript |
| `pyproject.toml` or `setup.py` | Python |
| `go.mod` | Go |
| `pom.xml` | Java (Maven) |
| `build.gradle` or `settings.gradle` | Java / Kotlin (Gradle) |
| `Cargo.toml` | Rust |
| `composer.json` | PHP |

Search commands:
- `find . -name "*.csproj" -not -path "*/obj/*"` — list all .NET projects
- `find . -name "package.json" -not -path "*/node_modules/*"` — list all Node projects
- `find . -name "go.mod"` — list Go modules

---

## Layer Topology Mapping

After identifying the stack, map the architectural layers by reading folder structure and project names.

### .NET / ASP.NET Core (Clean Architecture)

Typical folder/project pattern:

```
src/
  *.Domain/         → Domain layer (entities, value objects, interfaces)
  *.Application/    → Application layer (use cases, handlers, DTOs)
  *.Infrastructure/ → Infrastructure layer (EF Core, repositories, external services)
  *.Api/            → Presentation layer (controllers, minimal APIs, DI wiring)
tests/
  *.Domain.Tests/
  *.Application.Tests/
  *.Infrastructure.Tests/
  *.Api.Tests/ or *.IntegrationTests/
```

Call chain: `Controller → Handler/Service → Repository/Domain`

### Node.js / TypeScript (Layered / NestJS / Express)

Typical patterns:

```
src/
  controllers/ or routes/      → Presentation
  services/                    → Application / business logic
  repositories/ or data/       → Data access
  models/ or entities/         → Domain / data models
  middleware/                  → Cross-cutting (auth, logging)

test/ or __tests__/ or *.spec.ts / *.test.ts
```

### Python (FastAPI / Django / Flask)

```
app/
  api/ or routers/             → Presentation
  services/ or use_cases/      → Application
  models/ or schemas/          → Domain / DTO
  repositories/ or db/         → Data access
tests/
```

### Go

```
cmd/                           → Entry points (main packages)
internal/
  handler/ or api/             → Presentation
  service/                     → Application
  repository/ or store/        → Data access
  domain/ or model/            → Domain types
pkg/                           → Shared utilities
```

---

## Test Framework Detection

Search manifest files before assuming a framework. Never prescribe a framework not present in the codebase.

### .NET

Search `*.csproj` content:

| Keyword in `*.csproj` | Framework |
|---|---|
| `xunit` | xUnit (most common in modern .NET) |
| `nunit` | NUnit |
| `mstest` | MSTest |
| `FluentAssertions` | Assertion library (confirm present) |
| `NSubstitute` | Mocking library (confirm present) |
| `Moq` | Mocking library (confirm present) |

Command: `grep -r "xunit\|nunit\|mstest" --include="*.csproj"`

### Node.js / TypeScript

Search `package.json` `devDependencies`:

| Key | Framework |
|---|---|
| `jest` | Jest |
| `vitest` | Vitest |
| `@testing-library/react` | React Testing Library |
| `cypress` | Cypress (E2E) |
| `playwright` | Playwright (E2E) |
| `mocha` | Mocha |
| `chai` | Chai assertion library |

### Python

Search `pyproject.toml` or `requirements.txt`:

| Key | Framework |
|---|---|
| `pytest` | pytest (standard) |
| `unittest` | Built-in (stdlib) |
| `hypothesis` | Property-based testing |

---

## Naming Convention Discovery

Read 3–5 existing files in each layer before applying any naming rule. Extract the pattern in use — never assume.

### What to look for

| Convention | Where to look |
|---|---|
| Class naming | Existing entity, service, controller class names |
| Interface prefix | Does `I` prefix exist? (e.g. `IOrderRepository` vs `OrderRepository`) |
| File naming | `PascalCase.cs` vs `kebab-case.ts` vs `snake_case.py` |
| Test file suffix | `*.Tests.csproj`, `*.spec.ts`, `*_test.go`, `test_*.py` |
| Folder naming | `PascalCase/` vs `camelCase/` vs `kebab-case/` |
| Async suffix | Is `Async` suffix used on all async methods? |

---

## Entry Point Identification

Identify where execution begins — this is the starting point for tracing execution paths.

| Stack | Entry point |
|---|---|
| ASP.NET Core | `Program.cs` (or `Startup.cs` in older projects) |
| Node.js / Express | `index.ts` / `main.ts` / `app.ts` / `server.ts` |
| NestJS | `main.ts` (`NestFactory.create(...)`) |
| Python FastAPI | File containing `app = FastAPI()` |
| Python Django | `manage.py` → `settings.py` → `urls.py` |
| Go | `cmd/<app>/main.go` |
| Java Spring | Class annotated with `@SpringBootApplication` |

### HTTP entry points (API investigation)

After finding the entry point, trace to HTTP controllers/routes:
- .NET: Classes ending in `Controller` or files in `Endpoints/` / `Routes/`
- Express: `app.get(...)`, `router.post(...)` calls
- FastAPI: Functions decorated with `@app.get(...)` / `@router.post(...)`
- NestJS: Classes decorated with `@Controller(...)`

---

## Dependency Tree (quick map)

After reading the entry point, build a quick map:
1. List direct dependencies injected into the top-level controller/handler.
2. Follow one level deeper for each dependency.
3. Stop when you reach infrastructure (DB context, HTTP client) or a leaf service.

This produces a call chain useful for scoping implementation tasks and test boundaries.
