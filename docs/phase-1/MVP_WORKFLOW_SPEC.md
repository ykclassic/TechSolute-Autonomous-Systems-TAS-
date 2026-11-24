# TAS Phase 1 — Candidate MVP Workflow Specification

**Workflow:** Owner-approved post-appointment follow-up  
**Domain system:** SmeOps  
**Status:** Proposed; discovery and integration contract validation required  
**Risk:** R2 — customer-visible operational communication

## 1. Purpose

Prepare a consistent customer follow-up after a SmeOps appointment is completed, while keeping the business owner in control of whether it is sent. TAS prepares and governs the action; it does not autonomously send messages in the MVP.

## 2. Actors and systems

| Actor/system | Responsibility |
|---|---|
| SmeOps appointment service | Authoritative appointment, tenant and customer state |
| SmeOps event/trigger boundary | Announces or exposes completed appointment |
| TAS workflow | Validates context and creates action proposal |
| TAS policy service | Evaluates eligibility, risk, permission and approval |
| Business owner/authorized approver | Approves or rejects exact message |
| TAS action ledger | Records proposal, policy, approval and execution evidence |
| SmeOps Telegram outbound integration | Sends approved message through durable delivery path |
| Operator | Pauses workflow, investigates failure and reconciles unknown outcomes |

## 3. Trigger

Candidate trigger: appointment transitions to the authoritative completed state. Exact event name, transaction boundary and delivery mechanism are not verified. Phase 1 must confirm whether a durable event exists or an outbox/event contract is needed.

Do not trigger from a mutable UI label or untrusted client request.

## 4. Eligibility and validation

Before drafting, validate:
- Appointment exists and belongs to resolved tenant.
- Appointment is in authoritative completed state.
- Completion event is not stale, duplicated or superseded by correction.
- Customer association is valid within the same tenant.
- Active verified Telegram identity exists for the selected channel.
- Communication preference and opt-out rules permit the message.
- No prior follow-up for the same appointment/event has been dispatched.
- Required context is complete and appropriate for customer communication.

If a required condition cannot be established, stop and record a safe reason. Never guess tenant, customer, recipient or appointment state.

## 5. Message content

Use a deterministic, reviewed template in the first release. Optional AI wording may be evaluated later but cannot add facts or alter eligibility/recipient decisions. Include only minimum necessary information, such as business name and a neutral thank-you/follow-up prompt. Do not disclose sensitive service details without explicit product and privacy approval.

## 6. Workflow lifecycle

`TRIGGERED → CONTEXT_RESOLVED → DATA_VALIDATED → DRAFT_PREPARED → AWAITING_OWNER_APPROVAL → APPROVED | REJECTED | EXPIRED → DISPATCH_PENDING → DISPATCHED | FAILED | OUTCOME_UNKNOWN → RECONCILED`

Rules:
- Human approval is required for every customer-visible dispatch in MVP.
- Approval binds to exact payload hash, recipient, tenant, appointment reference, channel and expiry.
- Material change to message, recipient or appointment state invalidates approval.
- Revalidate appointment state, recipient eligibility and policy immediately before dispatch.
- Repeated event resolves to same idempotent action; no second dispatch.
- Ambiguous provider outcome enters `OUTCOME_UNKNOWN`; do not blindly retry.
- Rejection/expiry produces no external side effect.
- Operator can pause workflow and disable adapter.

## 7. Approval experience

Approver sees appointment reference and minimal context, customer display name and identity status, exact message, rationale, data validation/policy result, risk class, expected side effect and expiry.

The approval endpoint verifies authenticated owner authority server-side. Workflow initiator cannot approve its own high-risk action.

## 8. Integration contract

TAS must not write directly to SmeOps tables.

1. SmeOps emits a versioned completed event or exposes a stable authenticated API.
2. TAS consumes idempotently and resolves current domain state through supported contract.
3. TAS creates and approves its own action record.
4. TAS adapter invokes a supported SmeOps outbound service/API with idempotency key and tenant-scoped recipient reference.
5. SmeOps remains responsible for provider delivery, message ledger and channel rules.
6. TAS stores SmeOps action/message reference and delivery outcome, not duplicate domain state.

The current `enqueue_customer_appointment_notification` helper is an internal SmeOps function; it is not automatically a stable cross-repository API. SmeOps owner must approve an integration contract before TAS calls it.

## 9. Failure handling

| Failure | Required behavior |
|---|---|
| Missing actor/tenant context | Deny and record safe failure |
| Appointment not completed | Reject/defer; no dispatch |
| Appointment corrected/cancelled | Invalidate pending action and approval |
| Missing/ambiguous recipient | Block; never infer a chat |
| Preference/opt-out blocks | Block and record policy reason |
| Policy unavailable | Fail closed |
| Approval expired or payload changed | Require new approval |
| Duplicate event | Return existing action; no duplicate side effect |
| Adapter timeout with unknown provider outcome | Mark unknown and reconcile by idempotency/provider reference |
| Permanent delivery rejection | Record failure; retry only after classification |
| TAS pause/agent suspension | Stop new actions; preserve pending evidence |

## 10. Security and privacy

Resolve tenant from trusted server-side context. Use narrowly scoped service identity. Do not expose bot/provider credentials to agent/model processes. Do not log message bodies, phone numbers, Telegram IDs or unnecessary personal data. Store references/hashes where possible. Revalidate state before dispatch. Keep production dispatch disabled until release acceptance.

## 11. Outcome measures

Primary candidate KPI: owner time required to complete an eligible post-appointment follow-up.

Secondary measures: eligible completed appointments; draft preparation time; approval/rejection/expiry; confirmed dispatch/delivery; customer response or repeat booking in an agreed window; duplicate/unauthorized dispatch; cost per successful follow-up; opt-out, complaint and failed-delivery rates.

Baseline values are not available from repository review and must be collected before pilot claims.

## 12. Eventual MVP acceptance criteria

1. Only authoritative completed appointments trigger workflow.
2. Cross-tenant appointment/customer/recipient access is denied.
3. Unverified/ineligible recipients cannot be dispatched to.
4. Customer-visible dispatch always requires valid owner approval.
5. Approval binds to exact payload and expires.
6. Changed appointment state/payload invalidates approval.
7. Duplicate events/retries create no duplicate sends.
8. Ambiguous delivery is reconciled and never reported as confirmed without evidence.
9. Every action transition has actor, timestamp, tenant reference, correlation ID and policy/approval evidence.
10. Operators can pause workflow and disable dispatch.
11. Baseline and pilot KPI definitions are documented.
12. No production enablement before security, product and operations sign-off.

## 13. Open implementation questions

- Does SmeOps expose a durable completed-event boundary today?
- What is the authoritative completion state and correction policy?
- What exact customer communication preferences/opt-out controls apply to Telegram?
- Can the outbound path be exposed through a versioned service/API?
- What idempotency key is shared across TAS and SmeOps?
- What retention applies to drafts, approvals and delivery evidence?
- Who is the accountable SmeOps owner and who may approve customer communication?


## 14. Code-inspection refinement (2026-11-24)

The current SmeOps completion endpoint in `app/api/business.py` performs the confirmed-to-completed transition, records `appointment.completed` in `AuditEvent` and commits. The inspected handler does not enqueue a job or publish an event as part of that transaction. Therefore, the MVP cannot safely assume a durable completion trigger exists today.

**Required prerequisite:** SmeOps must add or expose a versioned appointment-completed integration contract. For reliable asynchronous delivery, prefer a transactional outbox written in the same transaction as the appointment state and audit event, then relay it idempotently. The TAS consumer must deduplicate using a stable event identifier. Do not poll internal audit rows as an undocumented substitute.

SmeOps already has a durable background-job primitive with tenant-scoped idempotency, payload fingerprinting, worker leases and dead-letter handling. Reuse is possible only through an approved boundary; direct cross-repository imports or direct writes to SmeOps tables are not acceptable platform contracts.

The selected code verifies Telegram identity for customer notification enqueueing, but the review did not establish a complete Telegram opt-out/preference model. This is a blocking policy question for customer follow-up. Do not infer that WhatsApp opt-out semantics apply to Telegram.
