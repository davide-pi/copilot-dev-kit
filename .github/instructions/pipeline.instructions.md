---
applyTo: "**/*.{yml,yaml}"
---

# CI/CD Pipeline Conventions

These rules apply to all pipeline files regardless of platform (Azure Pipelines, GitHub Actions, GitLab CI, etc.).

---

## Structure

- Split pipelines into **reusable templates** / **reusable workflows**; never put everything in a single monolithic file.
- One file = one concern: separate build, test, deploy, and release stages into dedicated templates.
- Templates must be independently composable and must not assume a specific caller context.
- Keep each pipeline or template file under 150 lines; split when it grows beyond that.

---

## Scripts over Platform Steps

- Prefer CLI scripts over platform-specific built-in tasks whenever a direct CLI equivalent exists (e.g., `dotnet build` over `DotNetCoreCLI@2` in Azure Pipelines, `run: npm ci` over `actions/setup-node` wrappers in GitHub Actions).
- Use platform tasks only for operations with no simple CLI equivalent (e.g., app-store publishing, code signing).
- Pin tool versions explicitly in scripts or via toolchain setup steps.

---

## Reusable Templates

- Extract any job or steps repeated across two or more pipelines into a shared template.
- Pass all environment-specific values as parameters; never hardcode them inside a template.
- Document every parameter with a description and a default value where applicable.
- Store templates in a dedicated directory (e.g., `.github/workflows/templates/`, `pipelines/templates/`).

---

## Readability

- Use a descriptive `displayName` / `name` on every step and job; never rely on auto-generated names.
- Group related steps using stages, named jobs, or step groups.
- Move inline scripts longer than ~10 lines to a separate script file (`.sh`, `.ps1`, `.py`); do not embed logic in YAML.
- Use `#` comments to explain *why* a step exists when it is not self-evident.
- Declare all variables in a dedicated `variables:` / `env:` block at the top of the file.

---

## Variables and Secrets

- Never hardcode secrets, tokens, or connection strings — reference them from secret stores, variable groups, or environment secrets.
- Use descriptive variable names that reveal intent: `PUBLISH_PROFILE_PROD`, not `PP` or `VAR1`.
- Define variables at the narrowest scope needed (pipeline → stage → job → step).

---

## Triggers and Conditions

- Always define explicit trigger conditions; never rely on implicit platform defaults.
- Use path filters to skip pipelines for irrelevant changes (docs, README, etc.).
- Protect deployment stages with approval gates or explicit manual triggers.
- Use branch filters to restrict production deployments to release branches and main/master.

---

## Fail-fast and Reliability

- Fail at the first error; never set `continue-on-error: true` without a comment explaining why.
- Begin every shell script with `set -euo pipefail` (bash) or `$ErrorActionPreference = 'Stop'` (PowerShell).
- Cache dependencies (NuGet, npm, pip, Maven, Gradle) keyed on the lockfile hash.
- Prefer shallow clones (`fetch-depth: 1`) unless full history is explicitly required.
- Always publish test results and code coverage as artifacts, regardless of pass/fail.

---

## Security

- Run pipelines with the least privilege required; never use admin or owner-level tokens for standard build jobs.
- Never print secrets in logs; avoid `echo`-ing sensitive values.
- Pin third-party actions, orbs, and task extensions to an immutable version (commit SHA or signed tag); never use `@latest` or floating version ranges.
- Validate artifact integrity before deploying using checksums or signed manifests.
- Scope `GITHUB_TOKEN` / service principal permissions to the minimum required by each job.
