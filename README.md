# TechSolute Autonomous Systems (TAS)

**Reliable, data-driven autonomous systems that execute business processes, produce measurable outcomes, and remain under enforceable human control.**

TAS is a proposed shared platform and product family for governed business automation across TechSolute Hub products. It is not a general-purpose agent with unrestricted access to customer systems.

## Project status

**Current stage:** Phase 1 — Discovery, product definition and system inventory  
**Documentation baseline:** Initial repository documentation is being established.  
**Implementation status:** No TAS platform implementation is claimed by this documentation.

The Phase 1 desk review identifies SmeOps as the proposed first proving ground and a post-appointment follow-up workflow as the candidate MVP. Customer discovery, baseline KPI collection and final workflow-owner approval remain validation gates.

## Mission

Build reliable, data-driven autonomous systems that execute business processes, produce measurable outcomes, and remain under enforceable human control.

## Strategic model

TAS separates two layers:

- **TAS shared platform:** identity and tenant context, data contracts, workflow orchestration, policy evaluation, human approvals, action evidence, execution adapters, observability and outcome measurement.
- **Domain products:** SmeOps, ProfitForge / Research, PatternView, MITROS, TechSERS and future products retain their own domain data, business rules, user experiences and systems of record.

TAS must not force all products into one database, runtime or monolith. Shared services are introduced only where reuse is demonstrated.

## First proving ground

SmeOps is the proposed first integration because its repository documents multi-tenant appointment workflows, Telegram customer identity verification, durable outbound messaging and owner notifications.

**Candidate workflow:** prepare a post-appointment customer follow-up draft after an appointment is marked completed; validate tenant, appointment and recipient context; require an authorized owner to approve the exact message; then dispatch through an approved SmeOps messaging path. The MVP must not send messages autonomously.

This is a proposal, not an implemented SmeOps feature. The integration contract and customer communication policy must be verified before implementation.

## Documentation map

| Document | Purpose |
|---|---|
| [Product and Engineering Strategy](docs/strategy/PRODUCT_AND_ENGINEERING_STRATEGY.md) | Mission, product model, architecture, shared services, boundaries and MVP |
| [10-Phase Roadmap](docs/roadmap/10_PHASE_ROADMAP.md) | Delivery sequence, phase outputs and exit gates |
| [Phase 1 Product Brief](docs/phase-1/PRODUCT_BRIEF.md) | Target users, problem, value proposition, objectives and scope |
| [Project Inventory](docs/phase-1/PROJECT_INVENTORY.md) | Repository-level system inventory and integration boundaries |
| [MVP Workflow Specification](docs/phase-1/MVP_WORKFLOW_SPEC.md) | Candidate workflow, actors, data, controls and KPI definitions |
| [Assumptions and Open Questions](docs/phase-1/ASSUMPTIONS_AND_OPEN_QUESTIONS.md) | Unvalidated assumptions, discovery questions and decisions needed |
| [ADR-001: Initial Architecture Shape](docs/architecture/decisions/ADR-001-initial-architecture-shape.md) | Modular-monolith-first decision |
| [ADR-002: First Proving Ground](docs/architecture/decisions/ADR-002-first-proving-ground.md) | SmeOps selection and workflow boundary |

## Non-negotiable engineering rules

1. Validate data before it influences consequential decisions.
2. Evaluate policy server-side before execution; an agent cannot authorize itself.
3. Bind human approval to the exact action payload, target, scope and expiry.
4. Enforce tenant isolation server-side on every domain access.
5. Record every action with a durable lifecycle and correlation identifier.
6. Use idempotency, bounded retries and reconciliation for external side effects.
7. Never report an unknown execution outcome as success or blindly retry it.
8. Keep production execution disabled until explicit security, product and operational gates pass.
9. Treat AI output as untrusted input; deterministic domain services remain authoritative.
10. Measure business value against a baseline and guardrails, not automation volume.

## Delivery rules

- Work from a dedicated branch and open a pull request against `main`.
- Require review and passing automated checks before merge.
- A merged change is not proof of production acceptance.
- Record material architecture, scope and risk decisions as ADRs.
- Complete each roadmap phase only when its evidence-based exit criteria pass.

## Current limitations

- This repository currently establishes the TAS documentation baseline; it does not yet contain a deployed TAS runtime.
- Existing-project inventory is based on repository READMEs, directory structures and selected architecture documents. It is not a complete security, code-quality or production audit.
- The proposed SmeOps workflow has not yet been validated through customer interviews or measured baseline data.
- Technology choices and service-level targets remain subject to Phase 1 discovery and later architecture decisions.

**Project owner:** TechSolute Hub  
**Document baseline:** 2026-11-24
