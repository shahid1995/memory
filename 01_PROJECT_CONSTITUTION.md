# StrikeNova — Project Constitution

This document contains the principles and constraints that should remain stable unless the project owner explicitly changes them.

## Product Principles

1. Build for real options traders, with NIFTY options as the primary focus.
2. Favor useful, explainable and evidence-based analytics over feature quantity.
3. Do not present unvalidated research as a reliable trading signal.
4. Keep the product architecture extensible so additional instruments and analytics can be added without repeated rewrites.

## Technology / Cost Principles

1. Prefer genuinely free, lifetime-free tools and services where practical.
2. Do not assume a paid market-data vendor is acceptable.
3. Do not introduce a paid dependency merely for convenience.
4. Evaluate larger-scale infrastructure only when required by measured usage or architectural need.

## Market Data Principles

1. Treat data licensing, redistribution and broker terms as first-class architectural constraints.
2. Prefer user/broker-authorized data flows where appropriate.
3. Do not expose or redistribute data in ways that violate applicable terms.
4. Normalize provider-specific data behind internal interfaces where feasible.

## Security Principles

1. Never put secrets in source code or project memory documentation.
2. Minimize exposure of broker credentials and tokens.
3. Separate authentication, authorization and trading permissions.
4. Treat live order execution as a high-risk subsystem requiring explicit controls and auditability.
5. Protect user data and financial information.

## Trading Architecture Principles

1. Paper trading and live trading must not be accidentally interchangeable.
2. Server-authoritative execution and portfolio state should be used where correctness requires it.
3. Market calculations should have clearly defined inputs, units, timestamps and failure behavior.
4. Financial calculations should be deterministic and testable.
5. Risk controls should be designed before convenience features.

## Engineering Principles

1. Inspect existing implementation before changing architecture.
2. Prefer extending existing components over creating duplicates.
3. Keep boundaries explicit between ingestion, normalization, analytics, execution and presentation.
4. Avoid premature complexity, but design clear extension points.
5. Preserve working behavior unless there is a deliberate, tested reason to change it.
6. Every important architectural decision should be documented.
7. Every significant feature should have tests appropriate to its risk.

## Product / UX Principles

1. Prefer simple workflows for traders.
2. Avoid cluttered dashboards that bury actionable information.
3. Important information should be visually prioritized.
4. Complex analytics should have explanations and context.
5. Mobile/responsive behavior should be considered for major user-facing workflows.

## Deployment Principle

Production deployment is controlled by the project owner. Development tooling must not deploy production changes automatically unless explicitly approved.

## Change Rule

A principle may be changed when new evidence, regulatory requirements, cost constraints or architectural realities justify it. Such a change must be recorded as an explicit decision in `05_DECISIONS.md`.
