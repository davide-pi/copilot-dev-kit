---
applyTo: "**/*.{jsx,tsx}"
---

# React Conventions

Apply in addition to `javascript-typescript.instructions.md`.

- Components: `PascalCase` — file name matches component name.
- Never use `async` in `useEffect` directly; extract an inner async function or use a custom hook.
- Prefer function components with hooks; never use class components in new code.
- Extract logic into custom hooks (`use` prefix) when a component exceeds ~50 lines or reuses stateful logic.
- Follow the Rules of Hooks: only call hooks at the top level; never inside conditions or loops.
- Declare `useEffect` dependencies exhaustively; never suppress the `exhaustive-deps` lint rule.
- Avoid prop drilling beyond two levels — use context or a state manager.
- Memoize expensive computations with `useMemo`; stabilise callbacks with `useCallback` only when necessary (not by default).
- Keep components pure: no side effects during render.
