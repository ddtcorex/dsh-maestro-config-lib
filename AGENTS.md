# AGENTS.md — dsh-maestro-config-lib

Foundation library of the Maestro shared-settings design (spec:
workspace `docs/specs/2026-08-26-dsh-maestro-settings-design.md`). Pure Node/TS,
zero Cordis dependency, embedded by every plugin that needs settings.

## Layout

- `src/index.ts` — store (lock/atomic io/deep merge), domain validators, legacy migration.
- `tests/{store,schema,migration}.spec.ts` — 19 tests, all contract-level.

## Rules

- Default branch `master`; no direct commits — `feat/<topic>` PRs; conventional commits.
- One TDD task = one commit; never commit red.
- Store format is versioned (`version: 1`); unknown domains/keys MUST survive round-trips.
- Writes are validated ONLY for domains registered via `defineDomain`; never validate foreign domains.
- Migration is one-shot and non-destructive: consumed legacy → `.bak`, corrupt → skipped untouched.
