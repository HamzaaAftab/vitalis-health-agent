# ADR-0011: LangSmith Cloud for tracing

- **Status:** Accepted
- **Date:** 2026-10-25
- **Phase:** 0

## Context

The docs planned Langfuse, self-hosted in docker compose. Self-hosted Langfuse v3 needs several extra containers (its own Postgres, ClickHouse, Redis, and S3-compatible storage). That is heavy for a development laptop, and Phase 0 makes no LLM calls, so there is nothing to trace yet. The project uses LangGraph, which LangSmith traces natively.

## Decision

Use **LangSmith Cloud** for LLM, node, and tool tracing in every environment. There is no tracing container in docker compose. Each environment uses its own LangSmith project and API key.

## Alternatives

| Option | Why not chosen |
|---|---|
| Self-hosted Langfuse | Heavy local footprint; more infrastructure to run before it is useful |
| Langfuse Cloud | Viable, but LangSmith has first-party LangGraph integration |
| Self-hosted LangSmith | Requires an enterprise licence |

## Consequences

- Traces leave our infrastructure. Until Phase 2 passes, only synthetic data is used, so this is acceptable for development.
- **Before real users (Phase 2 onward):** review the LangSmith data region, retention, and access control as part of the data-residency decision (architecture §19). Trace metadata must never hold PHI (guidelines §15).
- Tracing setup lives in `infrastructure/telemetry/`. Verify the LangSmith SDK configuration via context7 when it is implemented (Phase 1).
