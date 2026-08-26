# StrikeNova — Current State

**Last documented:** 2026-08-26

## Authoritative Status

The current implementation baseline is the remote application repository `shahid1995/-options-dashboard`, specifically feature branch `feat/phase-7-8a-historical-gex` at `bc9fe02679c669d5e7a547adfc1cee4fcf95881c`. Main remains separate at `ef46827e87fc6da4346606e3c754cab9a1c4732a` and PR #18 is open/draft/not merged.

A detailed reconciliation is recorded in `08_CURRENT_STATE_AUDIT_2026-08-26.md`.

## Verified Implementation Foundations

- Next.js/React frontend with existing authenticated app pages and public marketing pages.
- FastAPI backend with auth, option-chain, paper-trading, strategy, GEX, historical-GEX and candle routers.
- Upstox broker integration and broker-neutral gateway/adapter architecture.
- Paper trading, strategy templates/resolution, execution audit/idempotency, portfolio/performance and capital/margin foundations.
- Historical market-data pipeline covering NIFTY candles, option candles, contract metadata, candle validation/normalization/retry/coverage, strike/expiry selection, historical Greeks and daily ingestion.
- Historical GEX calculation, analytics, research and data-quality infrastructure.
- Database code supports PostgreSQL through `DATABASE_URL` and a deterministic SQLite fallback for local use.

## Research Status

Historical GEX research is validated as a research/structural analytics domain, not as a reliable standalone directional signal. The strongest current interpretation is structural/volatility-regime information.

Best Strike Selection, POS/gap prediction, scalping, IV history and option-price projection remain research/product-development areas until leakage-safe out-of-sample evidence supports productionization.

## Production Readiness

StrikeNova is **not yet production-ready for a 1,000-user commercial deployment**. Important gates remain around branch reconciliation, test-system reproducibility, data-pipeline operational verification, authentication/authorization/data isolation, rate limiting, observability/auditability and final infrastructure selection.

## Immediate Development Direction

FreeBuff's next work should be stabilization and verification, not new trading features:

1. Reconcile feature branch with current main without merging yet.
2. Verify repository integrity and runtime-artifact exclusion.
3. Stabilize and reproduce backend/frontend test execution.
4. Verify historical data-pipeline behavior safely and idempotently.
5. Perform the production security/architecture gate.
6. Decide infrastructure/database changes from measured requirements.

Only after these gates should major research/product work resume.

## Workflow State

- This conversation is the **StrikeNova Project Control Center**.
- `shahid1995/memory` is the long-term project-memory repository.
- Application implementation lives in `shahid1995/-options-dashboard`.
- FreeBuff may implement development changes, but production deployment remains under explicit/manual project-owner control.

## Constraints

- Prefer genuinely free/lifetime-free tools and services where practical.
- Do not assume paid market-data services.
- Do not introduce paid dependencies merely for convenience.
- Keep customer/broker-authorized market-data flows compliant with applicable terms.
- Keep paper/live/research boundaries explicit.
- Do not present unvalidated research as reliable trading signals.

## Reconciliation Rule

When memory conflicts with the actual application repository, the discrepancy must be recorded and resolved rather than silently choosing one source.
