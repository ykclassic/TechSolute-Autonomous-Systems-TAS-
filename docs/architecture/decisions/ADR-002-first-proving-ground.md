# ADR-002: SmeOps as the Proposed First TAS Proving Ground

- **Status:** Accepted as the first MVP direction; implementation gates remain open
- **Date:** 2026-11-24
- **Decision owner:** TechSolute Hub product leadership
- **Related phase:** Phase 1

## Context

TAS needs one bounded workflow to validate data checks, action proposals, policy, human approval, execution and measurable outcomes.

SmeOps documents tenant-scoped appointment/customer data, deterministic booking, verified Telegram customer identities and durable Telegram outbound processing. Other TechSolute products have distinct boundaries: Research and MITROS include financial/risk/execution concerns; PatternView is read-only; TechSERS is a separate Git infrastructure product.

## Decision

Select SmeOps as the first TAS proving ground. The user approved the MVP direction on 2026-11-24. This decision selects the workflow and does not represent completion of customer discovery, assignment of the SmeOps operational owner, approval of integration contracts or authorization for production dispatch.

Candidate MVP: owner-approved post-appointment follow-up.
1. Trigger from authoritative completed appointment.
2. Validate tenant, appointment, customer identity and communication eligibility.
3. Prepare deterministic follow-up draft.
4. Require owner approval bound to exact message and recipient.
5. Dispatch through supported SmeOps messaging boundary.
6. Record delivery and outcome evidence.

Telegram is the first channel candidate because verified identities and durable outbound delivery are documented. This is not final until preference, opt-out and privacy rules are confirmed. WhatsApp is not a Phase 1 dependency while production business-number acceptance is externally constrained.

## Alternatives considered

- **Research:** strong data/risk concepts, but financial execution raises risk and is unnecessary for first operational proof.
- **PatternView:** provenance and explainability, but read-only boundary does not validate controlled side effects.
- **MITROS:** useful event/approval patterns, but financial execution must remain separately governed.
- **TechSERS:** technically distinct infrastructure project, not a first-wave workflow.

## Consequences

Positive: bounded operational workflow; potential reuse of SmeOps tenant and outbound controls; avoids payments and financial execution.

Risks: customer need and willingness to pay are unvalidated; event and outbound contracts need code-level confirmation; consent/opt-out/privacy remain open; internal SmeOps functions are not automatically stable APIs.

## Approval record

- **Approval:** User approved the owner-approved Telegram follow-up MVP direction on 2026-11-24.
- **Scope:** One post-appointment follow-up workflow, Telegram as the conditional first channel, deterministic message template, mandatory owner approval for every dispatch.
- **Not approved by this decision:** autonomous customer messaging, production enablement, direct database access, polling internal audit rows as an integration contract, or bypassing SmeOps messaging controls.

## Conditions before implementation

- Named SmeOps owner approves workflow.
- Customer discovery and baseline KPI method are documented.
- SmeOps confirms supported event/API and idempotency behavior.
- Telegram recipient eligibility and opt-out behavior are verified.
- Security/privacy review approves data flow.

The workflow selection is accepted. Implementation remains blocked until the conditions below are evidenced and reviewed.
