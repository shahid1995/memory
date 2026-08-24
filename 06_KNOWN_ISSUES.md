# StrikeNova — Known Issues / Open Questions

This register contains items that require verification, resolution or explicit decision. It is intentionally conservative: an item is listed when the current documented context is insufficient to claim that it is resolved.

## Architecture / Implementation Verification

- Full current application-repository audit is still required.
- Current production topology needs verification.
- Exact current database schema needs verification.
- Exact current market-data providers/adapters need verification.
- Exact WebSocket/streaming implementation needs verification.
- Exact caching/background-worker topology needs verification.
- Exact authentication/authorization implementation needs verification.
- Current test/CI coverage needs verification.

## Product / Research

- GEX methodology requires continued validation before treating derived signals as predictive.
- POS/gap-prediction methodology requires independent validation.
- Scalping/high-confidence setup methodology requires rigorous historical/out-of-sample validation.
- Best Strike Selection needs a clearly defined scoring model and validation methodology.
- Option-price projection must communicate model assumptions and limitations.
- CE/PE ask-side and other microstructure data availability must be verified for the actual chosen data sources.

## Regulatory / Operational

- Live trading architecture and user-IP requirements require continued technical/legal validation against current applicable requirements.
- Broker data usage and redistribution terms must be verified per broker/provider before production use.
- Production security, secret management and auditability require formal review before live trading.

## Infrastructure

- Final free-tier hosting/database architecture has not been permanently selected.
- Scaling assumptions should be based on measured usage rather than speculative capacity planning.
