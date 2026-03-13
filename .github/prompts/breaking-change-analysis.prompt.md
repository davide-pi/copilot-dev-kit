---
name: analyze_breaking_changes
description: Analyse a diff or list of changes and identify breaking changes to public API surfaces, contracts, events, and serialization.
tools:
  - read
  - execute
  - search
  - vscode/askQuestions
  - web
---

# Breaking Change Analysis

## Step 1 – Gather the diff

Run `git diff origin/main...HEAD` (or `$BASE` if specified).
If the user provides a list of changes directly, use that instead.

## Step 2 – Identify the public surface

Scan for changes to:
- **Public types and members**: classes, interfaces, enums, structs, functions, constants that are exported or `public`/`protected`.
- **REST / GraphQL contracts**: endpoint paths, HTTP methods, request/response shapes, status codes.
- **Events and messages**: event names, payload schemas, topic/queue names.
- **Serialization formats**: JSON property names, binary formats, database column names (if mapped to DTOs).
- **Configuration keys**: environment variable names, `appsettings.json` keys used by consumers.
- **Package exports**: `index.ts` / `public API` surface for libraries.

## Step 3 – Classify each change

For every change found, classify it:

| Category | Breaking? | Example |
|----------|-----------|---------|
| Remove public member | ✅ Yes | Delete a method, property, or export |
| Rename public member | ✅ Yes | Rename without an alias |
| Change method signature | ✅ Yes | Add required parameter, change return type |
| Narrow accepted input | ✅ Yes | Accept fewer types, tighter validation |
| Change serialized name | ✅ Yes | Rename JSON property |
| Change default value | ⚠️ Maybe | Could affect callers that relied on the old default |
| Add optional member | ✅ No | New optional parameter with default |
| Widen return type | ✅ No | Return more specific subtype |

## Step 4 – Produce the report

```markdown
## Breaking Change Report

### ✅ Breaking changes

<!-- One entry per breaking change -->
**<Symbol or endpoint>** — `<file>:<line>`
- **Type**: <Removed / Renamed / Signature changed / Serialization changed / …>
- **Impact**: <Who is affected and what breaks>
- **Migration**: <What consumers must do to adapt>

---

### ⚠️ Potentially breaking changes

<!-- Changes that may break depending on consumer usage -->

---

### ℹ️ Non-breaking changes

<!-- Notable additions or widening changes -->

---

**Summary:** ✅ N breaking · ⚠️ N potentially breaking · ℹ️ N non-breaking
```

## Rules

- Base analysis strictly on the diff — do not speculate about unrelated areas.
- If a change is in a file marked `@internal`, `internal`, or `private`, note it but classify as non-breaking unless it is part of a public contract.
- Always include a migration path for every breaking change, even if it is simply "update callers".
