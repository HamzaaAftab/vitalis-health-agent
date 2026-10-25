# ADR-0012: A Supabase cloud project per environment (no local stack)

- **Status:** Accepted
- **Date:** 2026-10-25
- **Phase:** 0

## Context

The docs planned the Supabase CLI local stack (`supabase start`) for development. The project owner prefers a hosted Supabase project, which avoids installing the Supabase CLI and running its many containers on Windows.

## Decision

- Local development uses a dedicated Supabase **dev** cloud project (Postgres + Storage). Staging and production each get their own project, as before. Projects and keys are never shared across environments.
- The API connects through **Supavisor session mode** (IPv4). Migrations use the direct connection when the network has IPv6, otherwise session mode. (Supabase docs, "Connecting to Postgres": the direct connection is IPv6-only without the IPv4 add-on.)
- Alembic owns extensions, schemas, tables, RLS, **and** the `medical-files` bucket. There is no `supabase/` folder.
- Integration tests and the RLS check in CI run against **Postgres and Redis containers**, not the cloud. Tests marked `cloud` (Storage, bucket privacy, Data API exposure) run against the dev project.

## Alternatives

| Option | Why not chosen |
|---|---|
| Supabase CLI local stack | Owner's preference; extra tooling and containers on Windows |
| Cloud project for all tests | Slow, flaky, and shared state between test runs |

## Consequences

- Development needs an internet connection.
- The dev project holds **synthetic data only**.
- The Data API's exposed-schemas setting is a dashboard setting, not schema. The `cloud` test suite checks that `app` and `agent` are not exposed.
- CI needs the dev project's credentials as GitHub secrets to run `cloud` tests.
- The free plan limits the number of active projects. Staging may need a paid plan or a paused dev project later.
- The exact SQL or API for creating the bucket from a migration must be verified via context7 when it is implemented.
