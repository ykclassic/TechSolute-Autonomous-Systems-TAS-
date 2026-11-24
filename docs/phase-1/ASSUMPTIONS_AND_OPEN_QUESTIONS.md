# TAS Phase 1 — Assumptions, Open Questions and Decision Register

**Status:** Open working register | **Last reviewed:** 2026-11-24

## 1. Assumptions requiring validation

| ID | Assumption | Evidence currently available | Validation required |
|---|---|---|---|
| A-01 | Service businesses experience costly manual follow-up gaps | Product hypothesis only | Interview owners/operators; quantify frequency and cost |
| A-02 | SmeOps is best first proving ground | README, tree and selected Telegram/appointment docs | Confirm with product owner and estimate integration effort |
| A-03 | Post-appointment follow-up is valuable | Proposed workflow only | Confirm customer need, owner process and outcome |
| A-04 | Telegram is viable for customer follow-up | Verified identity and durable outbound delivery documented | Validate customer expectation, preference, opt-out and legal basis |
| A-05 | Appointment completion is a reliable trigger | Lifecycle capabilities documented | Inspect event transaction boundary and correction behavior |
| A-06 | SmeOps outbound delivery can be reused safely | Internal outbound service exists | Approve versioned cross-repository integration contract |
| A-07 | Shared control plane reduces duplication | Similar governance needs across products | Validate with a second use case before broad abstraction |

## 2. Product discovery questions

### Customer and buyer
1. Who experiences the follow-up problem: owner, receptionist, service staff or customer?
2. How many eligible appointments occur weekly per target business?
3. What proportion currently receives follow-up?
4. What is current preparation and sending time?
5. What is the measurable cost of missed/delayed follow-up?
6. Who pays and what budget funds it?
7. What tools or workarounds are used today?

### Workflow and channel
1. Is the goal satisfaction, feedback, rebooking or service recovery?
2. How soon after completion should a follow-up be sent?
3. Which appointment categories must be excluded?
4. How are complaints, disputed appointments and sensitive services handled?
5. Does the customer expect or consent to the selected communication?
6. How are opt-outs recorded and enforced?
7. Is Telegram representative of target customer behavior, or is WhatsApp essential commercially?

### Governance and operations
1. Which owner/manager roles may approve messages?
2. What evidence must approvers see?
3. How long should approval remain valid?
4. Who handles delivery failures and complaints?
5. What retention/deletion rules apply to drafts and action evidence?
6. What happens when TAS or SmeOps is unavailable?

## 3. Engineering questions

- What event/API is authoritative for appointment completion?
- Can SmeOps emit a durable event transactionally with appointment update?
- What is the supported integration boundary: signed webhook, internal API or message bus?
- How is service identity provisioned, rotated and revoked?
- What is the cross-system idempotency contract?
- How can an ambiguous Telegram send be reconciled without duplicate?
- Which system owns customer communication preferences and consent?
- What audit data may be retained, for how long, and who can read it?
- What are actual infrastructure and model costs per workflow?

## 4. Phase 1 acceptance checklist

| Requirement | Status | Evidence / next action |
|---|---|---|
| Mission and product boundaries documented | Evidence ready | `docs/strategy/PRODUCT_AND_ENGINEERING_STRATEGY.md` |
| Existing products inventoried | Desk review complete | `docs/phase-1/PROJECT_INVENTORY.md`; not a full code/security audit |
| Candidate MVP workflow specified | Drafted | `docs/phase-1/MVP_WORKFLOW_SPEC.md` |
| Named product/workflow owner confirmed | Open | Assign accountable owner |
| Customer discovery completed | Open | Record interviews and findings |
| Baseline KPI measured | Open | Collect eligible volume and manual follow-up time |
| SmeOps event/API contract verified | Open | Inspect appointment completion implementation/transaction |
| Channel eligibility and opt-out verified | Open | Review Telegram preference and product policy |
| Risks and non-goals documented | Evidence ready | Product brief and this register |
| Initial architecture decisions recorded | Evidence ready | ADR-001 and ADR-002 |

## 5. Phase 1 exit decision

**Decision: NOT ACCEPTED / IN PROGRESS.**

Desk-based repository inventory and documentation artifacts are ready for review. Phase 1 cannot be accepted until:
- An accountable product/workflow owner is assigned.
- Customer discovery is recorded.
- A primary KPI and baseline collection method are approved.
- SmeOps confirms the event and outbound integration contract.
- Telegram eligibility, opt-out and privacy requirements are confirmed.

If any item is intentionally deferred, the accountable owner must record the risk and approve the revised scope. Do not silently mark Phase 1 complete.
