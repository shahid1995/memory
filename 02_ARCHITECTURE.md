# StrikeNova — Architecture Baseline

> Initial architecture baseline. This is a living document and must be updated when architecture changes.

## 1. High-Level Architecture

StrikeNova should evolve as a layered system:

```text
User / Browser
      |
      v
Frontend / UX
      |
      v
Application API / Backend
      |
      +-------------------+
      |                   |
      v                   v
Domain / Analytics     Trading / Portfolio
Engines                Services
      |                   |
      +---------+---------+
                |
                v
        Data / Persistence
                ^
                |
      Market Data / Broker
      Integrations / Streams
```

The exact technology choices and deployed topology must be kept synchronized with the actual application repository.

## 2. Core Boundaries

### Frontend
Responsible for presentation, user workflows, interaction state and visualization. It should not become the authoritative source of financial/trading state where server authority is required.

### Backend / API
Responsible for business rules, authorization, server-authoritative state, APIs and coordination of domain services.

### Market Data Layer
Responsible for provider/broker-specific ingestion, normalization, timestamps, quality checks and delivery to downstream consumers.

### Analytics / Calculation Layer
Responsible for deterministic calculations such as Greeks, IV analytics, GEX and other research/product indicators. Calculations should be testable independently of the UI.

### Trading Layer
Responsible for paper/live order lifecycle, validation, risk controls, portfolio state and broker execution boundaries.

### Persistence Layer
Responsible for durable application state, user data, trading records, analytics data where persistence is justified, and audit/history requirements.

## 3. Data Flow Principle

External data should pass through explicit provider/integration boundaries before being consumed by internal domain logic.

Conceptually:

```text
Broker / Provider
      |
      v
Ingestion
      |
      v
Normalization
      |
      +------> Cache / Stream
      |
      v
Analytics Engines
      |
      +------> APIs
      |
      v
Frontend
```

## 4. Analytics Architecture

The analytics layer should support independent engines/modules rather than placing formulas directly into frontend components.

Known domains include:

- Greeks
- IV
- GEX
- POS/gap research
- OI/positioning analytics
- Vega/Delta/Gamma divergence
- Strike selection
- Option-price projection
- Scalping research
- Other validated trading analytics

Research indicators should be separated conceptually from production trading signals until validated.

## 5. Trading Architecture

Paper trading and live trading must have explicit boundaries.

The trading system should support:

- order intent
- validation
- execution
- fills
- positions
- P&L
- capital/margin
- audit/history
- broker integration

Live trading should require stronger authorization, risk controls and auditability than paper trading.

## 6. Capital / Margin

A capital and margin foundation has been implemented in the development history, including a server endpoint and provider abstraction. The exact current implementation must be verified in the application repository before making further architectural assumptions.

## 7. Database

The database is a persistent domain boundary, not merely a frontend data store. Schema decisions should consider:

- user ownership
- trading lifecycle
- historical records
- auditability
- indexes
- retention
- concurrency
- migration safety
- scale

The current database technology and complete schema should be recorded here after an application-repository audit.

## 8. WebSockets / Streaming

Real-time market data and live updates should be treated separately from ordinary request/response APIs where appropriate. Connection management, backpressure, reconnection, rate limits and data freshness should be explicit concerns.

## 9. Security

Security architecture should cover:

- authentication
- authorization
- secret management
- broker credentials/tokens
- API abuse/rate limiting
- audit logging
- data isolation
- live-order permissions
- secure deployment

## 10. Architecture Change Rule

Before adding a major feature, determine whether it belongs in an existing domain/service or requires a new bounded component. Avoid duplicating calculations, data access or business rules.

## 11. Known Architecture Gaps

These are intentionally marked as requiring repository verification rather than guessing:

- exact production deployment topology
- exact frontend/backend technology versions
- complete database schema
- exact market-data providers and broker adapters currently implemented
- exact WebSocket architecture
- exact caching/background-job topology
- current observability/monitoring stack
- current production environment

These should be filled from the actual application repository during the next architecture audit.
