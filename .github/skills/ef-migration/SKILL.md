---
name: ef-migration
description: Use when you need to create, update, or remove Entity Framework Core migrations or when you have to apply them to the database.
---

ALWAYS use `dotnet ef` commands — NEVER manually create or edit migration/snapshot files.

If a migration requires manual changes (e.g. custom SQL scripts), create it with `dotnet ef migrations add` first, then edit the generated file.

Use descriptive names reflecting the change: `AddCustomerEmail`, `RemoveObsoleteColumnInUsersTable`.
