---
applyTo: "**/*.ts"
---

# Angular Conventions

Apply in addition to `javascript-typescript.instructions.md`.

- Services, components, pipes, directives: `PascalCase` with suffix — `UserService`, `UserComponent`.
- Use `inject()` function instead of constructor injection for new components and services.
- Prefer `Signal`-based state (`signal`, `computed`, `effect`) over `BehaviorSubject` for local state.
- Use `OnPush` change detection strategy for all components.
- Unsubscribe from Observables using `takeUntilDestroyed()` or the `async` pipe; never leave subscriptions open.
- Services should be `providedIn: 'root'` unless explicitly scoped to a feature module.
- Avoid logic in templates beyond simple bindings; move conditions and transformations to the component class.
