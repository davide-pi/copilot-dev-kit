---
applyTo: "**/*.{js,ts,jsx,tsx}"
---

# JavaScript / TypeScript Conventions

These rules apply to all JS/TS files in addition to the language-agnostic rules in `copilot-instructions.md`.

---

## TypeScript — Strict Mode

- Always enable `"strict": true` in `tsconfig.json`; never disable individual strict flags.
- Avoid `any`; use `unknown` when the type is truly unknown and narrow with type guards.
- Never use non-null assertion (`!`) unless you have verified the value is non-null; add a comment explaining why.
- Prefer `interface` for object shapes that may be extended; use `type` for unions, intersections, and aliases.
- Use `readonly` on properties and array types that must not be mutated.
- Explicit return types on exported functions; inferred types are acceptable for non-exported helpers.

---

## Naming

- Variables, functions, parameters: `camelCase`.
- Classes, interfaces, type aliases, enums: `PascalCase`.
- Constants (module-level, never reassigned): `SCREAMING_SNAKE_CASE`.
- Boolean variables and functions: prefix with `is`, `has`, `can`, `should` — `isLoading`, `hasError`.

---

## Async / Await

- Always use `async/await` over raw Promises and `.then()` chains.
- Always handle rejections: wrap in `try/catch` or propagate explicitly.
- Type Promise results explicitly: `Promise<User>`, not `Promise<any>`.

---

## Import Order

Organise imports in this order, separated by a blank line:

1. Node built-ins (`fs`, `path`, `url`)
2. Third-party packages (`react`, `lodash`, `@angular/core`)
3. Internal aliases / path mappings (`@/components`, `@core/`)
4. Relative imports (`../utils`, `./helpers`)

Remove unused imports immediately; do not leave them commented out.

---

## Error Handling

- Always handle Promise rejections and Observable errors at the boundary (component, effect, interceptor).
- Never silently swallow errors with empty `catch` blocks.
- Use typed error handling: narrow caught errors with `instanceof` or type guards before accessing properties.

---

## Code Style

- Use `const` by default; use `let` only when reassignment is needed; never use `var`.
- Prefer template literals over string concatenation.
- Prefer destructuring for objects and arrays when accessing multiple properties.
- Use optional chaining (`?.`) and nullish coalescing (`??`) instead of verbose null checks.
- Trailing commas in multi-line structures (arrays, objects, parameters).
- No semicolons in projects configured without them; be consistent within the project.
