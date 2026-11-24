# TAS 10-Phase Delivery Roadmap

**Status:** Planning baseline. A phase is accepted only when its evidence is reviewed; merged code alone is not completion.

| Phase | Name | Outcome |
|---|---|---|
| 1 | Discovery, product definition and system inventory | Product brief, MVP workflow, system inventory, KPI plan and risk register |
| 2 | Platform foundations and engineering standards | Reproducible build/deploy, CI, environments, secrets and architecture standards |
| 3 | Identity, tenant isolation and agent registry | Verified actor/tenant context and scoped agent lifecycle |
| 4 | Data contracts, provenance and validation | Trusted decision inputs with data quality gates |
| 5 | Action lifecycle, policy engine and approvals | Enforceable authority, approval integrity and action state machine |
| 6 | Durable workflow orchestration and execution adapters | Reliable staging workflow and scoped adapter |
| 7 | Audit evidence, observability and operations | Traceability, monitoring, emergency controls and recovery |
| 8 | MVP product integration and controlled pilot | Domain integration and evidence of user/business value |
| 9 | Hardening, scale and commercial readiness | Security, performance, cost and support readiness |
| 10 | Platform expansion and productization | Stable contracts and second integration or evidence-backed no-go |

## Phase 1 — Discovery, product definition and system inventory

**Objective:** Validate scope and establish the current-state baseline across TechSolute products.

**Work:** confirm target customer/buyer and pain; inventory SmeOps, Research, PatternView, MITROS and TechSERS; select one workflow; map actors, data, state transitions, failure modes and KPIs; define boundaries, risk and non-goals.

**Deliverables:** product brief; MVP workflow specification; repository inventory; assumptions/open questions; initial ADRs.

**Exit:** named owner and workflow; all actors, inputs, side effects and systems of record identified; baseline KPI definitions and owners; risks recorded; customer validation documented.

**Current status (2026-11-24):** desk-based repository review and documentation produced; the user approved the owner-approved Telegram post-appointment follow-up as the first MVP direction. Customer interviews, named SmeOps workflow owner/approvers, baseline KPI data, event/outbox and outbound integration acceptance, and Telegram communication-policy validation remain open. Phase 1 is not yet accepted.

## Phase 2 — Platform foundations and engineering standards

**Objective:** Establish a deployable, testable foundation without premature service decomposition.

**Work:** repository conventions, CI and release workflow; API/event versioning and error taxonomy; isolated development/staging; secrets handling; migrations, backup/restore and rollback; threat model and data classification.

**Deliverables:** architecture standards, CI, staging deployment, environment controls and recovery procedure.

**Exit:** reproducible clean build/deploy; no production secrets in non-production; secret scanning; tested database restore; reviewed threat model.

## Phase 3 — Identity, tenant isolation and agent registry

**Objective:** Establish who acts and the exact scope of authority.

**Work:** human and workload identity; server-side tenant context; agent owner, purpose, version, status, allowed tools and risk class; suspension and permission audit.

**Deliverables:** identity contract, agent registry and access-control tests.

**Exit:** all defined cross-tenant and unauthorized tests pass; suspended/unregistered agents cannot act; permission changes are attributable.

## Phase 4 — Data contracts, provenance and validation

**Objective:** Prevent invalid, stale or misattributed data from driving consequential actions.

**Work:** versioned input contracts; schema, source, freshness, completeness, consistency and tenant validation; missing/stale/conflicting data behavior; provenance evidence.

**Deliverables:** contract catalogue, validation service/library, provenance model and fixtures.

**Exit:** decision inputs have documented sources and rules; invalid/stale/cross-tenant data blocks actions; each action links to input provenance.

## Phase 5 — Action lifecycle, policy engine and approvals

**Objective:** Make authority explicit before external side effects.

**Work:** canonical action envelope, payload hash and state machine; default-deny policy, risk tiers and limits; approval inbox; approval binding to exact payload, target, tenant, scope, expiry and policy version.

**Deliverables:** action lifecycle API, policy rules, approval service and tamper/replay tests.

**Exit:** unauthorized, expired, altered and replayed actions are blocked; proposer cannot approve own high-risk action; policy outage fails closed.

## Phase 6 — Durable workflow orchestration and execution adapters

**Objective:** Execute one narrow workflow reliably in staging.

**Work:** durable jobs/events, deduplication, idempotency, bounded retries, timeout and dead-letter handling; one scoped domain adapter; reconciliation for ambiguous outcomes; production execution remains disabled until separate approval.

**Deliverables:** workflow runtime, staging adapter and failure/recovery evidence.

**Exit:** duplicate delivery creates no duplicate side effect; ambiguous outcomes are not reported as success; operator can pause workflow and disable adapter.

## Phase 7 — Audit evidence, observability and operational readiness

**Objective:** Make actions inspectable and failures visible.

**Work:** append-only action evidence; correlated logs, metrics, traces, dashboards and alerts; runbooks; backup/restore, audit access and retention tests.

**Deliverables:** action ledger/evidence viewer, operations dashboard, alert rules and runbooks.

**Exit:** all test actions contain required audit fields; operators can trace end-to-end; pause and recovery drills pass.

## Phase 8 — MVP product integration and controlled pilot

**Objective:** Validate product integration and actual user value.

**Work:** versioned domain integration; minimal status and approval UX; staging acceptance; limited observe/recommend/prepare pilot; only specifically approved low-risk actions.

**Deliverables:** integration, pilot release candidate, UAT report and baseline-versus-pilot KPI report.

**Exit:** MVP acceptance passes; no unresolved critical/high security or data-integrity defects; measurable improvement or documented decision to revise use case.

## Phase 9 — Hardening, scale and commercial readiness

**Objective:** Prove operational and commercial supportability.

**Work:** threat assessment, dependency review and appropriate penetration testing; load testing; database/retention/cost measurement; quotas, metering, tenant administration and support; onboarding/offboarding and incident communications.

**Deliverables:** security/performance assessment, capacity/cost report and support model.

**Exit:** no unresolved critical vulnerabilities; high-risk findings have remediation plans; capacity/cost assumptions measured; support and recovery procedures tested.

## Phase 10 — Platform expansion and productization

**Objective:** Turn the validated slice into a reusable platform.

**Work:** publish stable APIs and integration guide; evaluate second integration; introduce governed templates only when controls remain enforceable; establish compatibility, deprecation, migration and ownership policy.

**Deliverables:** TAS v1 docs/integration kit; second integration or evidence-backed no-go; platform operating model.

**Exit:** second workflow reuses authorization/audit services without duplication, or an evidence-backed decision defers expansion; compatibility tests pass; owners approve v1 operations.

## Cross-phase release gates

1. Product: named owner, testable user problem and outcome.
2. Data: source, lineage, freshness and tenant validation.
3. Security: least privilege, access tests and no unresolved critical vulnerabilities.
4. Governance: independent policy, approval integrity and audit trail.
5. Reliability: idempotency, timeout, retry, reconciliation and recovery evidence.
6. Operations: monitoring, runbooks, pause controls and deployment verification.
7. Commercial: cost and support model before paid launch.

## Status vocabulary

- **Not started:** no implementation/evidence.
- **In progress:** work underway; exit criteria unmet.
- **Evidence ready:** artifacts and test evidence available for review.
- **Accepted:** accountable reviewers approved all exit criteria.
- **Blocked:** a dependency or failed gate prevents progress.

Never use "complete" to mean only "code merged".
