# StrikeNova — Project Status

**Purpose:** Living high-level status record for the StrikeNova project.
**Last updated:** 2026-08-27
**Status source:** StrikeNova Project Control Center + verified application-repository audits.

> This file is continuously updated. It records the current known state, not aspirations. A feature is not considered implemented merely because it was discussed or planned.

---

## 1. Overall Status

### 🟡 ACTIVE DEVELOPMENT — NOT PRODUCTION READY

StrikeNova has substantial trading, analytics, paper-trading, historical-data and GEX foundations. The project is now implementing the platform infrastructure required for secure multi-user commercial operation.

---

## 2. Application Baseline

**Application repository:** `shahid1995/-options-dashboard`
**Memory repository:** `shahid1995/memory`

Verified foundations include:

- Next.js/React frontend.
- FastAPI backend.
- Upstox OAuth integration.
- Session-based authentication and broker-token isolation.
- Broker-neutral gateway/adapter architecture.
- Paper trading, execution authorization/idempotency and capital/margin foundations.
- GEX and historical-GEX infrastructure.
- Historical market-data ingestion/validation.
- PostgreSQL support with local SQLite fallback.
- OAuth state protection, CORS controls, rate limiting and other production-readiness security work.

### Important limitation

The original authentication model was substantially broker/session-coupled. Phase 10 is migrating this toward a durable StrikeNova application identity while preserving the existing broker gateway and token store.

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
| GEX / historical GEX | 🟢 | Analytics/research foundations |
| Historical market data | 🟢 | Ingestion/validation foundations |
| Authentication | 🟡 | Existing auth is being evolved |
| Persistent StrikeNova user identity | 🟡 | Phase 10.1 implemented on PR #19; merge/CI pending |
| Persistent session/device management | 🟡 | Durable session ownership started; full device UX remains |
| RBAC / authorization | 🔴 | Phase 10.4 |
| Subscription plans | 🔴 | Phase 10.5 |
| Server-side entitlements | 🔴 | Phase 10.5 |
| User account portal | 🔴 | Phase 10.7 |
| Admin portal | 🔴 | Phase 10.8+ |
| Admin RBAC | 🔴 | Phase 10.4/10.8 |
| Formal audit/event system | 🟡 | Existing security/execution audit foundations; unified platform audit remains |
| Admin MFA | 🔴 | Phase 10.12 |
| Commercial onboarding readiness | 🔴 | Blocked by remaining identity/security/subscription gates |
| Production readiness | 🔴 | Broader security, operations and infrastructure gates remain |

---

## 4. Active Phase

### Phase 10 — User Management, Identity, Subscriptions & Admin Portal

**Status:** 🟡 In Progress

Authoritative plan:

`phases/PHASE-10-USER-MANAGEMENT-ADMIN-PORTAL.md`

### Phase sequence

1. 🟢 10.0 Repository audit/design freeze — completed for current implementation slice
2. 🟡 10.1 Persistent identity foundation — implemented on application PR #19; CI/merge pending
3. 🔴 10.2 Authentication/account access
4. 🔴 10.3 Session/device management (full UX/lifecycle)
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

## 5. Phase 10.1 — Current Implementation

### Implemented in application PR #19

Branch:

`feat/phase-10-identity-foundation`

Head commit:

`e6df4750f2db25b1eeb386fe5f45aaee56423684`

Added:

- Durable `users` identity table.
- Durable `user_sessions` table.
- Stable internal StrikeNova UUID user ID.
- Upstox broker identity mapping using broker/provider + broker user ID.
- Session ownership linked to persistent user ID.
- SHA-256 hashing of stored session identifiers; raw session IDs remain in the existing token store.
- `/auth/me` account identity endpoint with no broker-secret exposure.
- Durable session revocation on logout.
- Platform account suspension is not silently undone by broker login.
- Focused identity/session isolation tests.

### Important boundary

This is deliberately **not** the final Phase 10 system. It does not yet implement:

- customer email/password/passkey authentication,
- RBAC,
- subscriptions,
- entitlements,
- broker-connection persistence as the final domain model,
- user portal,
- admin portal,
- admin MFA,
- unified audit/event system.

### Validation

Tests were added, but the current tool environment did not provide a local checkout/runtime capable of executing the complete application test suite. GitHub CI is therefore the required validation gate before merge.

No production deployment was performed.

---

## 6. Target Architecture

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

Broker authorization must eventually be attached to a user-owned broker connection rather than defining the permanent application identity.

---

## 7. Commercial Assumptions

Current planned pricing discussed for StrikeNova:

- Monthly: ₹500
- Annual: ₹4,000
- Long-term target: 10,000+ users.

These remain product assumptions and must not be hard-coded into feature authorization.

---

## 8. Latest Session Handoff

### Discovered

The application already had substantial authentication/session, broker isolation, trading user-scoping, PostgreSQL and security foundations. The major missing platform layer was persistent customer identity and the management systems around it.

### Decided

User management and administration are being built as a unified identity/access platform layer, not as a disconnected admin feature. The existing broker gateway and token store are being extended rather than duplicated.

### Changed

- Created the complete Phase 10 start-to-finish plan.
- Created this living status file.
- Started application implementation of Phase 10.1.
- Created application PR #19 for the durable identity foundation.

### Tested

- Added focused identity/session tests.
- Full repository test suite has **not yet been executed in this environment**.
- GitHub CI is the next validation gate.

### Remaining

1. Validate PR #19 through CI and code review.
2. Merge only after tests pass and implementation is reconciled.
3. Continue Phase 10.2/10.3 authentication and session lifecycle design/implementation.
4. Build RBAC before exposing any admin functionality.
5. Continue through subscriptions, broker ownership, user portal and admin portal.

### Important risks

- Broker-coupled identity must be migrated without breaking existing sessions.
- Cross-user data and broker isolation is critical.
- Admin privilege escalation must be prevented.
- Subscription/entitlement enforcement must be server-authoritative.
- Broker secrets must never be exposed through user/admin interfaces.
- The current schema bootstrap approach must eventually be reconciled with a proper migration strategy before commercial production.

### Recommended next step

**Validate and merge Phase 10.1 first. Then begin Phase 10.2/10.3 rather than jumping directly to the admin UI.**

---

## 9. Status Rules

- `🔴 Not Started`
- `🟡 In Progress`
- `🟢 Complete`
- `🟠 Blocked`
- `⚪ Deferred`

Whenever a significant implementation session occurs:

1. Verify the application repository.
2. Update the relevant phase status.
3. Record what actually changed.
4. Record tests executed and results.
5. Record remaining work and risks.
6. Update `04_CURRENT_STATE.md` when the verified baseline changes.
7. Add accepted architectural decisions to `05_DECISIONS.md`.

The application repository remains the source of truth for what is actually implemented; this repository is the durable project memory.
