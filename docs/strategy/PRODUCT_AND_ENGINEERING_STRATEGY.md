# TAS Product and Engineering Strategy

**Version:** 1.0 | **Status:** Proposed baseline | **Owner:** TechSolute Hub | **Date:** 2026-11-24

## 1. Mission and vision

**Mission:** Build reliable, data-driven autonomous systems that execute business processes, produce measurable outcomes, and remain under enforceable human control.

**Vision:** Enable organizations to safely delegate well-defined operational work to intelligent software while retaining visibility, authority, accountability and the ability to intervene.

**Product promise:** Every consequential automated action should be explainable, permissioned, attributable and measurable. Operators must be able to identify the data used, why an action was proposed, which policy governed it, who approved it, what changed and what outcome followed.

## 2. Strategic thesis

The durable value is not an AI agent in isolation. It is trusted inputs, bounded decision-making, enforceable authority, reliable execution and evidence of business impact.

A TAS workflow must:
1. Receive a valid event or request.
2. Resolve actor, tenant, environment and resource context.
3. Validate decision-relevant data for source, schema, ownership, completeness and freshness.
4. Produce a typed action proposal through deterministic logic or a bounded agent.
5. Evaluate permission, scope, risk and approval requirements through an independent server-side policy.
6. Obtain authorized human approval when required.
7. Execute through a least-privilege adapter with idempotency and reconciliation.
8. Record decisions and state transitions.
9. Measure workflow and business outcomes.

Model confidence is not authorization. An audit log is not proof of correctness. Human approval does not replace validation.

## 3. Product model and boundaries

TAS has two layers.

### Shared platform
Identity and tenant context; agent registry; data contracts and provenance; durable workflow orchestration; policy evaluation; human approval; action lifecycle and evidence ledger; execution adapter contracts; observability; outcome measurement.

### Domain products
Each product remains authoritative for its own entities, business rules, calculations and user experience.

| Product | Potential TAS role | Boundary |
|---|---|---|
| SmeOps | Governed appointment and customer-communication workflows | SmeOps owns tenants, customers, appointments, business rules and channel policies. |
| ProfitForge / Research | Validated market-data workflows, decision evidence and analytics | Financial calculations, entitlements, risk logic and execution safeguards remain domain-owned. |
| PatternView | Provenance, provider health and explainable analysis | Read-only analysis remains domain-owned; no trade execution. |
| MITROS | Event, approval and controlled-execution patterns | Market intelligence, risk policy, proposals and execution gateway remain separately governed. |
| TechSERS | Possible future governed engineering workflows | Repository permissions, CI, human review and deployment gates cannot be bypassed. |

These are candidate integrations, not claims that TAS contracts already exist.

### Non-goals
TAS is not an unrestricted general-purpose agent, universal database, mandatory common runtime, autonomous financial execution platform, replacement for human accountability, or claim of regulatory certification. Existing products are not required to migrate into one monolith.

## 4. Initial customer hypothesis

The initial segment hypothesis is small and medium-sized service businesses with repetitive, measurable operational workflows: appointment follow-up, customer response, status updates and service administration. Buyer, urgency, current workaround, willingness to pay and measurable value require customer discovery.

## 5. Reference architecture

| Layer | Responsibility |
|---|---|
| Product interfaces | Product dashboards, approval inboxes, admin console and API clients |
| Domain applications | Product-specific entities, rules, workflows and systems of record |
| Integration boundary | Versioned APIs/events, signed webhooks, schemas, idempotency and adapters |
| TAS control plane | Identity context, agent registry, policy, approvals, action lifecycle and emergency controls |
| Workflow/execution plane | Durable orchestration, retries, timeouts, adapters, compensation and reconciliation |
| Data/evidence plane | Provenance references, action ledger, audit evidence, metrics and outcome records |
| Infrastructure/security | Secrets, encryption, backups, deployment, monitoring and incident response |

Start with a modular monolith or a small number of services. Do not create a microservice for every logical component before scaling, isolation or ownership requires it. Keep policy and approval enforcement server-side and independent of prompts and UI state. Separate development, staging and production credentials, databases and integrations.

## 6. Shared service contracts

| Service | Minimum contract |
|---|---|
| Identity and tenant context | Every request resolves verified actor, tenant, environment and correlation context. |
| Agent registry | Unregistered, suspended or out-of-policy agents cannot initiate actions. |
| Data validation | Inputs carry source, schema, tenant, timestamp, freshness and validation evidence. |
| Workflow service | Durable state, deduplication, retries, timeouts, dead-letter handling and recovery. |
| Policy decision point | Server-side allow/deny based on actor, action, resource, risk, limits and environment; default deny. |
| Approval service | Approval bound to exact payload hash, action, target, tenant, scope, approver and expiry. |
| Action ledger | Validated state transitions and append-only evidence for proposed, blocked, approved, executing, succeeded, failed and unknown actions. |
| Execution adapters | Least-privilege side effects with idempotency, bounded retries and reconciliation. |
| Outcome measurement | KPI definitions include owner, numerator, denominator, period, source and attribution caveats. |
| Observability and incident controls | Correlated logs, metrics, traces, alerts, pause controls and runbooks. |

### Canonical action envelope
Minimum conceptual fields: `action_id`, `tenant_id`, `initiator`, `workflow_id`, `action_type`, `resource_reference`, `payload_reference`, `payload_hash`, `data_provenance`, `policy_context`, `risk_class`, `execution_mode`, `approval_requirement`, `idempotency_key`, `created_at`, `expires_at`, `correlation_id`. Do not copy secrets or unnecessary personal data into payloads or logs.

### Action lifecycle
`PROPOSED → VALIDATING → BLOCKED | AWAITING_APPROVAL | AUTHORIZED → EXECUTING → SUCCEEDED | FAILED | OUTCOME_UNKNOWN → RECONCILING`

Rejected, expired and revoked are controlled terminal states. Compensation is a mitigating action, not a guaranteed rollback.

## 7. Human oversight and risk tiers

| Tier | Example | Default authority |
|---|---|---|
| R0 — Read-only | Retrieve, summarize, calculate | Scoped automatic execution |
| R1 — Reversible, low impact | Create internal draft or label | Automatic only after workflow-specific validation and policy approval |
| R2 — Material operational | Customer-visible communication or booking change | Human approval by default during MVP |
| R3 — High impact | Payment, refund threshold, contract, access change, production deployment or financial order | Explicit human approval; dual approval where policy requires |
| R4 — Prohibited / separately governed | Unlawful action or safeguard bypass | Blocked by policy |

Required controls: least privilege; separation of proposer and approver for high-risk actions; default deny on missing context or policy outage; payload-bound approval; state revalidation before execution; pause, suspension and credential revocation; append-only evidence; prompt-injection defenses; and production enablement only after explicit release gates.

## 8. MVP

**Objective:** Prove one staging workflow from trigger to data validation, typed action proposal, policy decision, human approval, scoped adapter execution and complete evidence.

**Candidate:** SmeOps post-appointment follow-up. When an appointment becomes completed, validate tenant, appointment, customer identity and communication eligibility; prepare a follow-up draft; require owner approval of the exact message; dispatch only through an approved SmeOps messaging boundary; record delivery and outcome evidence.

Telegram is the first channel candidate because SmeOps documents verified Telegram identities and durable outbound delivery. This remains subject to communication preference, opt-out and privacy review. WhatsApp is not an initial dependency while business-number acceptance remains externally constrained. Start with deterministic templates. AI drafting may be evaluated later but cannot decide eligibility, recipient, appointment state, approval or dispatch authorization.

**Exclusions:** autonomous customer sends; payments/refunds/trades/contracts/access changes/production deployment; arbitrary agents/tools; multi-agent orchestration; universal data lake; cross-product database; workflow builder before core controls are proven.

## 9. Reliability, security and data standards

Validate source, schema, freshness, completeness, consistency, uniqueness and tenant ownership. Assume at-least-once delivery and make consumers idempotent. Use bounded retries with jitter; do not blindly retry ambiguous external side effects. Use transactional outbox/inbox where state and event publication must remain consistent. Redact secrets and personal data. Define retention separately for domain data and action evidence. Monitor workflow completion, denials, approval latency, retries, queue age, adapter latency, cost and outcomes.

## 10. Success framework

Measure workflow completion, validated input rate, approval compliance, unauthorized side effects (target zero), duplicate side effects (target zero in acceptance tests), human time per eligible workflow, cost per successful workflow, customer response/rebooking, intervention and failure rates. Set targets after baseline measurement; do not claim causal impact from a simple before/after comparison.

## 11. Engineering and change control

Requirements map to tests and evidence. Code requires review and passing CI. Migrations are reviewed and tested. Production deployment is separate from merge acceptance. Security, tenant isolation, idempotency and failure-mode tests are release blockers. Promote shared abstractions only after real use demonstrates reuse. Changes to mission, product boundaries, risk tiers, production permissions, retention or release gates require an ADR.

This strategy does not certify existing products or assert that TAS services have been implemented.
