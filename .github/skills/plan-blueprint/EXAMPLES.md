# Plan Blueprint — Example

Fully-worked fictional plan demonstrating every element of the format.

## Plan: Add Notification Preferences Feature

**Goal** — Allow users to configure per-channel notification preferences stored persistently in the database.
**Total Effort** — 8 SP
**Risk Summary** — 🟢 1 · 🟡 1 · 🟠 1 · 🔴 0

**Assumptions**
- The `users` table already exists with a primary key `id`.
- The project uses Rails 7 with PostgreSQL.
- No existing notification settings are stored elsewhere that would conflict.

---

### **Step 1 – [🟢 LOW] – Add `NotificationPreferencesService`**

**What** — Create `app/services/notification_preferences_service.rb` implementing two public methods: `get(user_id, key)` returning the stored value or a default, and `set(user_id, key, value)` upserting a row. Use `UserPreference.find_or_initialize_by` internally. No controller wiring yet.

**Why** — Encapsulates all preference logic in one place before any API or UI layer touches it. Steps 2 and 3 both depend on this service existing.

**Files** — `app/services/notification_preferences_service.rb` · `spec/services/notification_preferences_service_spec.rb`

**Risk** — 🟢 LOW · Pure Ruby service with no external dependencies; trivially reversible by deleting the file. **Mitigation**: None required.

**Effort** — 2 SP

**Verify**
⚙️ `bundle exec rspec spec/services/notification_preferences_service_spec.rb`
👁️ Confirm both `get` and `set` are callable from a Rails console without errors.

---

### **Step 2 – [🟡 MEDIUM] – Expose preferences REST endpoint**

**What** — Add `GET /api/v1/users/:id/preferences` and `PATCH /api/v1/users/:id/preferences` routes to `config/routes.rb`. Create `app/controllers/api/v1/user_preferences_controller.rb` with `show` and `update` actions that delegate to `NotificationPreferencesService`. Return JSON with `{ key, value }` shape. Require authenticated session via `before_action :authenticate_user!`.

**Why** — Exposes the service created in Step 1 to clients. Step 3 (UI) requires this endpoint to exist and be tested before the frontend is wired up.

**Files** — `config/routes.rb` · `app/controllers/api/v1/user_preferences_controller.rb` · `spec/requests/api/v1/user_preferences_spec.rb`

**Risk** — 🟡 MEDIUM · Adding a public endpoint widens the attack surface; missing auth guard would expose preferences cross-user. **Mitigation**: Enforce `authenticate_user!` and add a request spec asserting 401 for unauthenticated requests.

**Effort** — 3 SP

**Verify**
⚙️ `bundle exec rspec spec/requests/api/v1/user_preferences_spec.rb`
👁️ Hit `GET /api/v1/users/1/preferences` with and without a session cookie; confirm 200 vs 401 respectively.

---

### **Step 3 – [🟠 HIGH] – Add `user_preferences` database migration**

**What** — Create `db/migrate/TIMESTAMP_add_user_preferences.rb` defining a `user_preferences` table: `user_id BIGINT FK → users.id NOT NULL`, `key VARCHAR(255) NOT NULL`, `value TEXT`, unique index on `(user_id, key)`. Also write the `down` method dropping the table. Run `rails db:migrate` to apply.

**Why** — The service (Step 1) and controller (Step 2) both reference the `UserPreference` model; this migration is the last piece that makes the full stack functional end-to-end.

**Files** — `db/migrate/TIMESTAMP_add_user_preferences.rb` · `db/schema.rb`

**Risk** — 🟠 HIGH · Schema migrations are hard to reverse in production without a compensating migration; a malformed constraint can corrupt existing rows. **Mitigation**: Write and verify the `down` method before merging; run against staging first.

**Effort** — 3 SP

**Verify**
⚙️ `rails db:migrate && rails db:rollback && rails db:migrate`
👁️ Open a DB console and confirm the `user_preferences` table exists with the correct columns, foreign key, and unique index.

---

## Dependency Diagram

```
           [Start]
              │
    ┌─────────┴─────────┐
    │                   │
  Step 1             Step 3
  Service           Migration
    │                   │
  Step 2                │
  Endpoint              │
    └─────────┬─────────┘
              │
           [Done]
```

---

## Summary

| Step | Risk | Effort | Key File | Change |
|------|------|--------|----------|--------|
| 1 | 🟢 LOW | 2 SP | `app/services/notification_preferences_service.rb` | Create service with `get`/`set` |
| 2 | 🟡 MEDIUM | 3 SP | `app/controllers/api/v1/user_preferences_controller.rb` | Create REST endpoint with auth guard |
| 3 | 🟠 HIGH | 3 SP | `db/migrate/TIMESTAMP_add_user_preferences.rb` | Schema migration with rollback |
