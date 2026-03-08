Document the decision to use runtime configuration loading in a structured ADR, so future developers understand the context, options, and consequences.

---

- **[Create ADR folder]** Add `docs/architecture/decision-records/` (or `docs/achtitecture/decision-records/` if matching the solution layout) and create `runtime-configuration.md`.

---

- **[Context section]** Describe the problem: Angular apps need environment-specific config (API endpoints, feature flags). Build-time config forces a separate build per stage and increases release overhead. Capture the goals: avoid boilerplate, allow same build in multiple stages, and provide type-safe access.

---

- **[Considered options]** Present at least two alternatives in a table, e.g. "Load when needed" vs "APP_INITIALIZER", with short descriptions and downsides for each.

---

- **[Decision section]** Summarize the chosen approach: load config at runtime via `APP_INITIALIZER`, expose it through a typed signal-based service, use external JSON, and inject the service where needed.

---

- **[Consequences]** List benefits (one build, type safety, no boilerplate) and any trade-offs (blocks startup until config is loaded).

---

- **[Header metadata]** Include status (Accepted), last update date, and optionally author or ticket references.
