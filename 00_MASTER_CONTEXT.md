# StrikeNova — Master Context

> Authoritative high-level project context. This document is the starting point for reconstructing StrikeNova context in a new session. Detailed decisions and research should be recorded in the dedicated documents in this repository.

## 1. Project

**Name:** StrikeNova

**Purpose:** Build a robust web application for options traders, initially focused on Indian index options, with analytics, research tools, paper trading, broker integration, alerts, trading journal, and eventually live trading capabilities.

## 2. Primary Focus

- NIFTY options are the primary focus.
- FINNIFTY, BANKNIFTY and other supported instruments may be considered where useful.
- The platform should combine market data, options-chain analytics, Greeks, volatility analytics, positioning analytics, trading tools and execution capabilities.

## 3. Core Architectural Principle

The conversation named **StrikeNova Project Control Center** is the working discussion space. This repository is the project's long-term memory and documentation source of truth.

The conversation should not be treated as the permanent source of architectural truth. Important decisions, current state, research conclusions and constraints must be promoted into this repository.

## 4. Permanent Constraints / Preferences

- Prefer genuinely free, lifetime-free tools and services where practical.
- Avoid paid market-data vendors unless explicitly approved.
- Prefer customer-owned broker/API connectivity where appropriate rather than redistributing broker data.
- Broker/API credentials and secrets must be protected and must not be exposed unnecessarily through the website.
- Live trading, paper trading, research and analytics should have appropriate separation and server-authoritative behavior where required.
- Do not deploy production changes automatically through implementation tooling unless explicitly approved; manual deployment is preferred by the project owner.
- Architecture should prioritize security, correctness, maintainability, scalability and performance rather than short-term implementation speed.

## 5. Known Major Product Areas

The project has discussed or implemented foundations for:

- Options dashboard / option chain
- Watchlist and alerts
- Greeks and IV analytics
- GEX / Gamma Exposure analytics
- POS-style gap prediction research
- Scalping / high-confidence small-point trade research
- Best Strike Selection for option buyers
- Option-price projection at a target index level
- Trading Journal
- Paper trading
- Capital and margin
- Broker integration
- Live order execution architecture
- Market-data and WebSocket architecture
- Security and SEBI-related technical considerations
- Hosting / infrastructure / database architecture

These areas should not automatically be considered production-complete merely because they were discussed. Current implementation status must be verified against the actual application repository.

## 6. Development Workflow

The preferred lifecycle for a significant feature is:

1. Problem definition
2. Current-state audit
3. Research / evidence gathering
4. Requirements
5. Alternatives and trade-offs
6. Architecture decision
7. Documentation update
8. Implementation plan
9. Implementation by the development workflow/tooling
10. Tests and validation
11. Review
12. Update project memory/current state

## 7. Architecture Review Rules

For significant changes, consider impacts on:

- Frontend
- Backend
- Database
- API contracts
- Market-data ingestion
- WebSockets / streaming
- Calculation engines
- Caching
- Background jobs
- Authentication and authorization
- Security
- Performance
- Scalability
- Testing
- Observability
- Deployment
- UX
- Future extensibility

Challenge assumptions when a proposed approach is weak, unnecessarily complex, insecure, expensive, difficult to scale, or inconsistent with existing architecture.

## 8. Financial / Research Rules

For trading analytics and research:

- Define formulas and inputs precisely.
- Define units, timestamps and frequency.
- Define missing-data and edge-case behavior.
- Distinguish live/model/theoretical values.
- Separate hypothesis from evidence.
- Do not claim predictive value without validation.
- Prefer historical testing and measurable validation before production adoption.

## 9. Known Development History

The project has progressed through several documented phases, including:

- Foundation / repository audit
- Risk calculation foundation
- Price-domain payoff work
- Scenario and time analysis
- Greek foundation and live-vs-model analytics
- IV analytics and volatility data foundation
- Generic Greek/IV statistical condition engine
- Server-authoritative paper trading and portfolio foundation
- Performance analytics
- Capital and margin foundation

The exact current phase and implementation status must be reconciled with the current application repository and recorded in `04_CURRENT_STATE.md`.

## 10. Important Existing Concepts

Examples of analytics and strategy concepts previously explored include:

- GEX
- Vega
- Delta
- Gamma
- Theta
- IV
- India VIX relationships
- CE/PE divergence
- Delta Dominance Index (DDI)
- Dealer inventory flip
- Gamma anomaly
- OI-based positioning
- Short covering / long unwinding / long buildup / short buildup
- Institutional flow / trap detection
- CE vs PE ask-side information

These are research/product concepts, not automatically validated trading signals.

## 11. Repository Boundaries

This repository (`shahid1995/memory`) is for StrikeNova project memory, architecture, research, decisions, roadmap, status and related documentation.

Do not store:

- API keys
- passwords
- access tokens
- database credentials
- production secrets
- private customer information
- other sensitive credentials

## 12. Working Principle

The objective is not merely to implement individual features. The objective is to build the best coherent StrikeNova system over the long term.

When uncertain:

- verify,
- research,
- inspect the current implementation,
- identify assumptions,
- explain trade-offs,
- and document important decisions.
