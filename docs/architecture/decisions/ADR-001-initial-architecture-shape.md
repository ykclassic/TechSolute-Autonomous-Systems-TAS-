# ADR-001: Modular-Monolith-First Architecture

- **Status:** Accepted as an initial planning constraint
- **Date:** 2026-11-24
- **Decision owner:** TechSolute Hub engineering leadership
- **Related phase:** Phase 1 / Phase 2

## Context

TAS needs logically distinct capabilities: identity, tenant context, agent registry, data validation, workflow orchestration, policy, approval, action ledger, adapters and observability. Deploying every capability as a separate service before usage, scaling and team boundaries are understood adds network failure modes, deployment complexity and operating cost before one workflow is proven.

## Decision

Start with a modular monolith or a small number of independently deployed components. Enforce internal module boundaries and versioned contracts even if modules share deployment or database.

Extract a capability only when a distinct security boundary, independent scaling/availability, separate team ownership, deployment cadence or validated external contract justifies it.

Policy and approval enforcement must remain server-side and independent of agent prompts and client UI.

## Consequences

**Positive:** lower initial infrastructure burden; faster end-to-end validation; easier local/staging testing; services can be extracted later.

**Negative:** requires disciplined module boundaries; shared deployment can couple releases/failures; database ownership must remain explicit.

## Guardrails

- Domain products remain systems of record for their entities.
- TAS modules do not directly mutate domain databases.
- Use explicit interfaces and integration contracts.
- Modular monolith is not permission to bypass policy or audit boundaries.
- Revisit after first workflow and second integration are evaluated.
