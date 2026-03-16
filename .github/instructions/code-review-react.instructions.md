---
applyTo: "**/*.{jsx,tsx}"
---

# Code Review – React

Apply with the severity model from `code-review.instructions.md`.

**React**
- Hook inside condition, loop, or nested function (Rules of Hooks) → **[CRITICAL]**
- Direct state mutation → **[CRITICAL]**
- Missing dependency in `useEffect`/`useCallback`/`useMemo` → **[IMPORTANT]**
- Missing `key` on `.map()` items → **[IMPORTANT]**
- Prop drilling beyond two levels → **[SUGGESTION]**

Use `typescript` for `.tsx`, `javascript` for `.jsx` in all code blocks.
