---
applyTo: "**/*.{js,ts,jsx,tsx}"
---

# Code Review – JavaScript / TypeScript

Apply the conventions from `javascript-typescript.instructions.md` with the severity model from `code-review.instructions.md`.

## Severity assignments

**TypeScript Strictness**
- `any` without justification, `!` without comment, `@ts-ignore`/`@ts-expect-error` without explanation → **[IMPORTANT]**
- Missing explicit return type on exported functions → **[SUGGESTION]**

**Naming**
- Variables/functions not `camelCase`, classes/interfaces/components not `PascalCase` → **[IMPORTANT]**
- Boolean identifiers missing `is`/`has`/`can`/`should` prefix → **[SUGGESTION]**

**Async / Await**
- Unhandled Promise rejection (no `try/catch` or `.catch()`) → **[IMPORTANT]**
- `async` function passed directly to `useEffect` → **[IMPORTANT]**
- Raw `.then()`/`.catch()` instead of `async/await` → **[SUGGESTION]**

**Variable Declarations**
- `var` anywhere → **[IMPORTANT]**
- `let` where `const` suffices → **[SUGGESTION]**

**Security**
- User-controlled data in `innerHTML`/`dangerouslySetInnerHTML`/`eval()` → **[CRITICAL]** (XSS)
- Hardcoded secrets or tokens → **[CRITICAL]**
- `console.log` with sensitive data in production → **[IMPORTANT]**

**React**
- Hook inside condition, loop, or nested function (Rules of Hooks) → **[CRITICAL]**
- Direct state mutation → **[CRITICAL]**
- Missing dependency in `useEffect`/`useCallback`/`useMemo` → **[IMPORTANT]**
- Missing `key` on `.map()` items → **[IMPORTANT]**
- Prop drilling beyond two levels → **[SUGGESTION]**

**Angular**
- Observable subscription not unsubscribed → **[IMPORTANT]**
- Business logic in template expressions → **[IMPORTANT]**
- `ChangeDetectionStrategy.Default` where `OnPush` suffices → **[SUGGESTION]**
- Service not `providedIn: 'root'` without explicit scope justification → **[SUGGESTION]**

**Error Handling**
- Empty `catch` block → **[IMPORTANT]**
- Caught error accessed as `any` without type narrowing → **[IMPORTANT]**

**Imports**
- Unused imports, wrong order (Node built-ins → third-party → internal → relative) → **[SUGGESTION]**

Use `typescript` for `.ts`/`.tsx`, `javascript` for `.js`/`.jsx` in all code blocks.
