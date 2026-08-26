# StrikeNova — Known Issues / Open Questions

This register contains items that require verification, resolution or explicit decision. It is intentionally conservative.

## Application / Git

- PR #18 is open/draft/not merged.
- Feature branch `feat/phase-7-8a-historical-gex` has been reconciled with current `main` without merging the PR; it is now 38 commits ahead and 0 behind `main`.
- `options-dashboard-project/docs/PROJECT_STATUS.md` contains older phase/deployment claims and should not be treated as the sole current status authority.

## Architecture / Implementation

- Final production database topology must be verified from the actual deployment environment; code supports PostgreSQL via `DATABASE_URL` and SQLite locally.
- Complete schema/migration strategy needs a formal production review.
- WebSocket/live-streaming architecture needs targeted reconciliation against current code and deployment rather than relying on older documentation.
- Authentication and authorization are not yet sufficient to claim production-grade multi-user isolation.
- API rate limiting, observability and auditability require production review.
- CI/test execution should be made reproducible and full-suite status should be separated from environment-specific collection failures.

## Data Pipeline

- Historical candle/option/Greeks/GEX pipeline is implemented on the feature branch, but continuous production operation and scheduled ingestion still require operational verification.
- Data licensing, provider limits, broker terms and redistribution constraints must be verified before commercial use.
- Production data must remain separate from development/test writes.
- `backfill_orchestrator.py` is present and is designed as a CLI-only, resumable, idempotent historical ingestion orchestrator; its safe operational execution still requires verification against non-production/test data.

## Product / Research

- GEX methodology should not be presented as a standalone directional predictor; current research supports structural/volatility-regime use.
- POS/gap-prediction methodology requires independent leakage-safe validation; the overnight-gap blueprint is a research specification, not an implemented production signal.
- Scalping/high-confidence setup methodology requires rigorous historical and out-of-sample validation.
- Best Strike Selection needs a defined scoring model, outcome definitions and validation methodology.
- Option-price projection must document assumptions and limitations.
- IV historical collection remains incomplete/disabled until data quality and storage bounds are verified.
- CE/PE ask-side and other microstructure data availability must be verified for the chosen data source.

## Regulatory / Operational

- Live trading architecture and user-IP requirements require continued technical/legal validation against current applicable requirements.
- Broker data usage and redistribution terms must be verified per broker/provider before production use.
- Production security and live-order authorization require formal review before enabling live execution.

## Infrastructure / Cost

- Final free-tier hosting/database architecture has not been permanently selected.
- Do not migrate databases solely because the SQLite file is large; use measured workload and architectural requirements.
- Do not introduce paid market-data or infrastructure dependencies merely for convenience.
