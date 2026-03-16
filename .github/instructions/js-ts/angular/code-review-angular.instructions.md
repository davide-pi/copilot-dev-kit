---
applyTo: "**/*.ts"
---

# Code Review – Angular

Apply with the severity model from `code-review.instructions.md`.

**Angular**
- Observable subscription not unsubscribed → **[IMPORTANT]**
- Business logic in template expressions → **[IMPORTANT]**
- `ChangeDetectionStrategy.Default` where `OnPush` suffices → **[SUGGESTION]**
- Service not `providedIn: 'root'` without explicit scope justification → **[SUGGESTION]**

Use `typescript` code blocks in all findings.
