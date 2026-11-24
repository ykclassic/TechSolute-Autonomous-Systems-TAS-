# TAS Phase 1 — TechSolute Project Inventory

**Snapshot:** 2026-11-24  
**Method:** GitHub repository metadata, default-branch READMEs, recursive file trees and selected architecture/design documents.  
**Scope:** Desk-based integration inventory, not a complete code audit, security assessment or production-readiness certification.

## Summary

| Repository | Observed product boundary | TAS relevance | Initial posture |
|---|---|---|---|
| [SmeOps](https://github.com/ykclassic/SmeOps) | Multi-tenant SME appointment and retail operations assistant | Bounded operational workflow | Proposed first integration |
| [research / ProfitForge](https://github.com/ykclassic/research) | Market research, data validation, signals, portfolio/risk and execution-related capabilities | Provenance, policy and evidence patterns | Read-only architecture study |
| [PatternView](https://github.com/ykclassic/PatternView) | Explainable crypto chart-pattern analysis; read-only | Data quality, provenance and explainability | No execution integration |
| [MITROS](https://github.com/ykclassic/MITROS) | Modular market-intelligence and trading operating system | Event, approval, risk and execution boundaries | Reuse patterns only; separately governed |
| [TechSERS](https://github.com/ykclassic/TechSERS) | Rust Git infrastructure derived from Aspen | Possible future governed engineering workflows | Not a first-wave runtime dependency |

## SmeOps

Repository: `ykclassic/SmeOps`, default branch `main`. Observed areas: `app/api`, `app/models`, `app/services`, `app/jobs`, `app/integrations`, `migrations`, `frontend`, `docs`.

README documents FastAPI, Next.js, PostgreSQL, SQLAlchemy/Alembic, tenant-scoped owners/services/customers/appointments, persisted jobs and a separate worker. It also documents a shared Telegram platform bot, tenant-specific booking links, verified Telegram customer identity and durable Telegram outbound messages/owner alerts. WhatsApp onboarding and outbound functionality exist, but production business-number acceptance remains subject to Meta onboarding/verification.

Selected evidence:
- `docs/telegram-platform-bot-architecture.md`: shared bot, opaque tenant references, tenant-scoped context and durable owner alerts.
- `docs/design/telegram-appointment-management.md`: verified customer identity, explicit confirmations, idempotent lifecycle notifications and tenant-safe appointment operations.
- `app/services/telegram_customer_notifications.py`: checks active platform bot and verified tenant-scoped customer identity before enqueue.
- `app/jobs/telegram_outbound.py`: durable outbound processing and ambiguous delivery handling.

**TAS opportunity:** propose follow-up action after authoritative appointment completion; TAS owns proposal/policy/approval evidence; SmeOps owns appointment/customer/messaging state.

**Questions:** confirm completed-event transaction boundary; Telegram preference/opt-out behavior; supported outbound API/service contract; completion correction/reversal handling; no sends to unverified/ambiguous recipients.

Status: strongest candidate; no TAS integration exists.

## ProfitForge / Research

Repository: `ykclassic/research`, default branch `main`. Tree includes market-data providers, validation, candle freshness/session logic, execution persistence, risk management, signal outcomes, billing/entitlements and security workflows.

README documents provider adapters, canonical quote validation and explicit unavailable states rather than fabricated fallback prices. Selected code `backend/app/services/execution.py` rejects live execution under its Phase 1 safety policy and constrains broker adapter use. Full current execution policy must be reviewed before any future integration.

**TAS opportunity:** provenance, freshness, policy decisions and auditable outcomes. Keep market data, signal calculations, risk qualification, entitlements and execution safeguards domain-owned. TAS must not turn research signals into orders or become an alternate financial execution path.

Status: architecture study only; not selected as MVP.

## PatternView

Repository: `ykclassic/PatternView`, default branch `main`. README and `docs/ARCHITECTURE.md` define a read-only crypto chart-pattern product. Provider adapters normalize candles; detection is deterministic/versioned; interpretation is separate; API preserves provenance. It does not connect to trading accounts or submit orders.

**TAS opportunity:** shared data provenance and explainable evidence. Boundary: pattern detection and interpretation remain product-owned; no trade execution.

Status: no initial integration planned.

## MITROS

Repository: `ykclassic/MITROS`, default branch `main`. Tree includes `contracts`, `packages`, `apps/web`, `docs/architecture` and CI workflows.

README describes phased market data, features, regime, strategy, consensus, risk, proposal and execution architecture. `docs/architecture/architecture.md` describes Vercel, Render and Supabase responsibilities. `docs/architecture/event-model.md` defines immutable events, correlation/causation IDs, schema versions and idempotent consumers. `docs/architecture/trade-proposal-human-approval.md` documents a human approval boundary separate from execution authorization.

**TAS opportunity:** study event contracts, human approval, immutable decisions and execution separation. Boundary: MITROS owns its trading domain, risk engine and execution gateway; TAS must not bypass them.

Status: architecture study only; financial execution remains separately governed.

## TechSERS

Repository: `ykclassic/TechSERS`, default branch `main`. The checked-in README identifies the project as Aspen, a self-hosted Git server, and describes pre-alpha status. Cargo workspace contains Rust crates for core, auth, metadata, blob, pack, planner, compaction, server and CLI.

**TAS opportunity:** possible future governed engineering workflows such as code analysis or test orchestration. Boundary: do not assume this repository is a TAS agent runtime or that its product scope has already changed. Confirm strategic direction before integration.

Status: no initial integration planned.

## Cross-cutting observations

1. Existing products already have domain-specific controls; TAS should integrate through explicit contracts rather than reimplement them.
2. SmeOps has the clearest bounded operational workflow for a first pilot.
3. MITROS provides event and approval patterns, but financial execution must remain separately governed.
4. PatternView and Research reinforce provenance and explicit unavailable/stale states.
5. TechSERS is architecturally distinct and outside the first delivery sequence.

## Verification still required

This inventory does not verify deployed commit parity, live database/migration state, runtime access policies, secrets, backups, retention, incident readiness, complete API compatibility, customer consent/legal basis, performance or unit economics. These require targeted review before integration or production release.
