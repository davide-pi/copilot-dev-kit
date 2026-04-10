---
applyTo: "**/*.cs"
---

## Entity Framework Migrations

**ALWAYS** use the `dotnet ef` CLI to create and apply migrations. **NEVER** create or modify migration files by hand.

### Allowed commands

```bash
# Create a new migration
# Use --output-dir to specify the migrations folder.
# If migrations already exist in the project, infer the folder from their location
# rather than using the default. Do not guess — check first with `dotnet ef migrations list`
# or by inspecting existing migration files.
dotnet ef migrations add <MigrationName> [--output-dir <MigrationsFolder>] [--project <project>] [--startup-project <startup>]

# Apply pending migrations to the database
dotnet ef database update [--project <project>] [--startup-project <startup>]

# Revert to a specific migration
dotnet ef database update <TargetMigration>

# Remove the last unapplied migration
dotnet ef migrations remove

# List all migrations and their applied status
dotnet ef migrations list

# Generate a SQL script instead of applying directly
dotnet ef migrations script
```

### Prohibited

- Creating `*_MigrationName.cs` or `*_MigrationName.Designer.cs` files manually.
- Writing `Up()` / `Down()` method bodies without first generating the migration via `dotnet ef migrations add`.
- Editing the auto-generated `ModelSnapshot` file directly.

### If customisation is needed

Generate the migration first with `dotnet ef migrations add`, then edit the resulting `Up()`/`Down()` methods as needed. Never skip the generation step.
