# TAS Phase 1 — Product Brief

**Version:** 1.0 | **Status:** Proposed; customer and owner validation required | **Date:** 2026-11-24

## Product definition

TAS is a governed automation platform that enables domain products to prepare and execute bounded business actions using validated data, server-side policy, human authorization and auditable outcomes. It is not a standalone chatbot. It provides reusable control and evidence services while preserving each domain product's system of record.

## Mission

Build reliable, data-driven autonomous systems that execute business processes, produce measurable outcomes, and remain under enforceable human control.

## Problem hypothesis

Small and medium-sized businesses may manage repetitive customer and operational workflows through disconnected dashboards, messaging channels and manual follow-up. This can lead to delayed responses, missed follow-ups, inconsistent records and poor operational visibility.

This is a hypothesis, not a validated customer finding. Discovery must establish frequency, workaround, cost, urgency and willingness to pay.

## Initial target segment

Primary hypothesis: appointment-based and service businesses in Nigeria, initially businesses already using SmeOps.

Potential users: business owner/manager, operations staff and customers receiving appointment-related communication. Potential buyer: business owner/operator. Buyer, budget and purchase criteria remain unvalidated.

## Proposed value

Reduce manual work in defined workflows without removing human authority. For the candidate workflow: improve follow-up consistency, reduce message preparation time, preserve owner control, record every decision and measure customer response/repeat booking. No savings or revenue uplift is claimed before measurement.

## First proving ground

SmeOps is proposed because its repository documents tenant-scoped appointment data, appointment lifecycle operations, shared Telegram bot, verified customer identity and durable outbound delivery. SmeOps remains the system of record for tenants, customers, services, appointments and messaging state. TAS must not duplicate those entities.

## Candidate MVP

**Owner-approved post-appointment follow-up assistant.** When an appointment reaches completed state, validate tenant and recipient context, prepare a follow-up draft, require owner approval of the exact message and dispatch only through an approved SmeOps messaging boundary.

Telegram is the initial channel candidate, subject to customer preference, opt-out and privacy review. WhatsApp is not an initial dependency while business-number acceptance remains externally constrained. Use deterministic templates first; AI drafting is optional later and cannot decide eligibility, recipient, state or authorization.

## MVP scope

Included:
- One tenant-scoped workflow and typed action proposal.
- Data and recipient eligibility validation.
- Default-deny policy evaluation.
- Human owner approval bound to exact message payload.
- One staging-only adapter or approved integration contract to SmeOps durable outbound messaging.
- Action evidence, correlation IDs and basic outcome metrics.
- Operator pause and failure visibility.

Excluded:
- Autonomous customer-message dispatch.
- Payments, refunds, trading, contracts, access changes or production deployment.
- Arbitrary agents/tools, multi-agent orchestration or cross-product data federation.
- Universal data lake or workflow builder before core contracts are proven.

## Success measures

| Measure | Definition | Baseline |
|---|---|---|
| Eligible completed appointments | Completed appointments meeting agreed eligibility rules | To collect |
| Follow-up preparation time | Median human time to prepare equivalent follow-up manually | To collect |
| Draft generation rate | Valid drafts / eligible appointments | Pilot |
| Approval rate | Approved drafts / drafts presented | Pilot |
| Dispatch success | Confirmed delivered messages / approved dispatch attempts | Pilot |
| Duplicate dispatches | Duplicate customer-visible sends for one workflow event | Target 0 in acceptance suite |
| Unauthorized dispatches | Sends without valid policy and approval | Target 0 |
| Customer response/rebooking | Defined response or repeat booking in agreed attribution window | To define |
| Cost per successful follow-up | Runtime, model/provider and support cost / successful follow-ups | Pilot |

Every KPI requires denominator, time window, source, exclusions and owner. Before/after movement alone does not establish causality.

## Key risks

- Appointment completion may not imply permission for follow-up.
- Customer may lack verified Telegram identity or may not expect communication.
- Appointment status may change after draft or approval.
- Replayed events/retries may duplicate messages.
- External delivery may be ambiguous.
- Approval fatigue can cause rubber-stamping.
- Message content may disclose sensitive service details.

Mitigate through explicit eligibility, verified recipient, revalidation before dispatch, payload-bound approval, idempotency, reconciliation, minimal content and human approval.

## Phase 1 validation required

1. Named SmeOps product/domain owner and approver.
2. Representative owner/customer discovery conversations or equivalent documented evidence.
3. Current follow-up process and baseline preparation time.
4. Authoritative completed-event and correction/reversal behavior.
5. Telegram communication eligibility, opt-out and customer-expectation rules.
6. Exact outbound API/service contract and transaction boundary.
7. Primary outcome KPI and attribution window.
8. Security/privacy review of data shared with TAS.

## Phase 1 status

- Desk research: completed for repository-level product fit.
- Product hypothesis: documented.
- Workflow specification: drafted.
- Customer validation: open.
- Baseline KPI collection: open.
- Final MVP approval: open.

Phase 1 remains in progress until open validation items are resolved or explicitly accepted as deferred risks by the accountable owner.
