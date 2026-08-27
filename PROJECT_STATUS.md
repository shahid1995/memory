# StrikeNova — Project Status

**Purpose:** Living high-level status record for the StrikeNova project.  
**Last updated:** 2026-08-27  
**Status source:** StrikeNova Project Control Center + verified project-memory/application-repository audits.

> This file is continuously updated. It records the current known state, not aspirations. A feature is not considered implemented merely because it was discussed or planned.

---

## 1. Overall Status

### 🟡 ACTIVE DEVELOPMENT — NOT PRODUCTION READY

StrikeNova has substantial trading, analytics, paper-trading, historical-data and GEX foundations. The project is now moving toward the platform infrastructure required for secure multi-user commercial operation.

The next major platform capability being planned is:

> **Identity + User Management + Subscriptions/Entitlements + Broker Connection Ownership + User Portal + Admin Portal**

This is documented as **Phase 10**.

---

## 2. Current Application Baseline

**Application repository:** `shahid1995/-options-dashboard`  
**Memory repository:** `shahid1995/memory`

The application currently has, based on the latest documented repository audit:

- Next.js/React frontend.
- FastAPI backend.
- Existing authenticated application pages.
- Upstox OAuth integration.
- Session-based authentication.
- Per-session broker-token isolation improvements.
- Broker-neutral gateway/adapter architecture.
- Paper trading and portfolio foundations.
- Execution authorization/idempotency/audit foundations.
- Capital/margin foundation.
- GEX and historical-GEX infrastructure.
- Historical market-data ingestion and validation infrastructure.
- PostgreSQL support with local SQLite fallback.
- Security hardening including OAuth state protection, CORS controls, rate limiting and production-readiness checks.

### Important limitation

The current authentication model is still substantially coupled to broker OAuth/session identity. A durable StrikeNova application identity is not yet fully established.

Therefore the project should **not** treat the current authentication/session implementation as the final commercial user-management architecture.

---

## 3. Platform Readiness Snapshot

| Area | Status | Notes |
|---|---|---|
| Frontend foundation | 🟢 | Existing Next.js/React application |
| Backend foundation | 🟢 | Existing FastAPI architecture |
| PostgreSQL foundation | 🟢 | Existing persistent DB support |
| Broker integration | 🟢 | Upstox + broker-neutral architecture |
| Paper trading | 🟢 | Existing foundation |
| Capital/margin | 🟢 | Existing foundation |
| GEX / historical GEX | 🟢 | Research/analytics foundations exist |
| Historical market data | 🟢 | Ingestion/validation foundations exist |
| Authentication | 🟡 | Existing, but broker-coupled model needs evolution |
| Persistent StrikeNova user identity | 🔴 | Phase 10 |
| Persistent session/device management | 🔴 | Phase 10 |
| RBAC / authorization model | 🔴 | Phase 10 |
| Subscription plans | 🔴 | Phase 10 |
| Server-side entitlements | 🔴 | Phase 10 |
| User account portal | 🔴 | Phase 10 |
| Admin portal | 🔴 | Phase 10 |
| Admin RBAC | 🔴 | Phase 10 |
| Formal audit/event system | 🟡 | Existing security/execution audit foundations; unified platform audit remains |
| Admin MFA | 🔴 | Phase 10 |
| Commercial onboarding readiness | 🔴 | Blocked by identity/security/subscription gates |
| Production readiness | 🔴 | Still requires broader security, operational and infrastructure gates |

---

## 4. Active Phase

### Phase 10 — User Management, Identity, Subscriptions & Admin Portal

**Status:** 🟡 Planned / architecture specification created  
**Implementation:** Not started

Authoritative plan:

`phases/PHASE-10-USER-MANAGEMENT-ADMIN-PORTAL.md`

### Phase 10 sequence

1. 🔴 10.0 Repository audit/design freeze
2. 🔴 10.1 Persistent identity foundation
3. 🔴 10.2 Authentication/account access
4. 🔴 10.3 Session/device management
5. 🔴 10.4 RBAC/authorization
6. 🔴 10.5 Subscription/entitlements
7. 🔴 10.6 Broker connection ownership/security
8. 🔴 10.7 User account portal
9. 🔴 10.8 Admin portal foundation
10. 🔴 10.9 Admin user management
11. 🔴 10.10 Subscription administration
12. 🔴 10.11 Audit/security events
13. 🔴 10.12 Admin MFA/high-risk controls
14. 🔴 10.13 Notifications/lifecycle automation
15. 🔴 10.14 Observability/operations
16. 🔴 10.15 Migration/rollback
17. 🔴 10.16 Comprehensive testing/security validation
18. 🔴 10.17 Production-readiness gate

---

## 5. Target Architecture Direction

The target commercial model is:

```text
StrikeNova Account
       |
       +-- Identity / Authentication
       +-- Sessions / Devices
       +-- Roles / Permissions
       +-- Subscription / Entitlements
       +-- Broker Connections
       +-- Preferences
       +-- Security / Audit History
       |
       +---- Existing Trading / Analytics Systems
```

### Critical separation

```text
StrikeNova User
      ≠
Broker Account
```

A user should be able to have a StrikeNova account independently of whether a broker is currently connected.

Broker authorization should be attached to a user-owned broker connection and should not define the permanent application identity.

---

## 6. Commercial Product Assumptions

Current planned pricing discussed for StrikeNova:

- Monthly: ₹500
- Annual: ₹4,000
- Long-term target: 10,000+ users.

These are product assumptions, not implementation facts. Pricing must remain configurable and must not be hard-coded into frontend feature checks.

---

## 7. Rules for Updating This File

Whenever a significant implementation session occurs:

1. Verify the application repository state.
2. Update the relevant phase status.
3. Record what actually changed.
4. Record tests executed and results.
5. Record remaining work.
6. Record important risks/blockers.
7. Update `04_CURRENT_STATE.md` when the verified project baseline changes.
8. Add architectural decisions to `05_DECISIONS.md` when a decision is formally accepted.
9. Never mark work complete based only on discussion.

### Required status language

- `🔴 Not Started`
- `🟡 In Progress`
- `🟢 Complete`
- `🟠 Blocked`
- `⚪ Deferred`

---

## 8. Latest Session Handoff

### Discovered

The application already contains significant authentication/session, broker isolation, trading user-scoping, PostgreSQL and security foundations. The major missing platform layer is persistent customer identity and the management systems around it.

### Decided for planning

User management and admin functionality should be built as a unified platform identity/access layer rather than as an isolated admin feature.

The admin portal should use the same authoritative backend identity/authorization model as the customer application.

The broker gateway and existing authentication/security foundations should be extended rather than duplicated.

### Changed

- Created the complete Phase 10 start-to-finish plan.
- Created this living project status file.

### Tested

No application code was changed in this session. No application tests were run.

### Remaining

- Perform the Phase 10.0 implementation-repository audit immediately before coding.
- Produce/finalize the detailed architecture and data-model specification from the current application schema.
- Approve the migration strategy before changing authentication identity.
- Implement Phase 10 incrementally with tests at every gate.

### Important risks

- Broker-coupled identity must be migrated without breaking existing sessions.
- Cross-user data/broker isolation is critical.
- Admin privilege escalation must be prevented.
- Subscription/entitlement enforcement must be server-authoritative.
- Broker secrets must never be exposed through user/admin interfaces.

### Recommended next step

**Start Phase 10.0 only:** perform a fresh read-only audit of the application repository's authentication, database schema, user ownership, broker token handling, frontend guards, migrations and tests. Do not implement schema changes until that audit is reconciled with this Phase 10 plan.

---

## 9. Project Control Rule

The StrikeNova Project Control Center conversation is the working coordination space. This repository is the durable memory.

The application repository is the source of truth for what is actually implemented.

When documentation and implementation disagree, record and reconcile the discrepancy rather than silently choosing one.
