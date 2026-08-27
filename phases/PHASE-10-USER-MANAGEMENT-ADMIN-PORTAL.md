# StrikeNova — Phase 10: User Management, Identity, Subscriptions & Admin Portal

**Status:** Planned — architecture/design work not yet implemented  
**Owner:** StrikeNova Project Control Center  
**Application repository:** `shahid1995/-options-dashboard`  
**Memory repository:** `shahid1995/memory`  
**Created:** 2026-08-27  

---

## 1. Purpose

Build the complete identity, account, authorization, subscription/entitlement, broker-connection ownership, user self-service and administration foundation required to operate StrikeNova as a secure multi-user SaaS product.

This phase must extend the existing architecture rather than create duplicate authentication, broker, database or business-rule systems.

The target model is:

```text
StrikeNova Account
        |
        +-- Identity / Authentication
        +-- Sessions / Devices
        +-- Roles / Permissions
        +-- Subscription / Entitlements
        +-- Broker Connections
        +-- User Preferences
        +-- Security / Audit History
        |
        +-------------------------------+
                                        |
                              Existing Trading,
                         Analytics & Market Systems
```

### Core architectural principle

**StrikeNova identity must be independent of broker identity.**

The long-term model is:

```text
StrikeNova User
      |
      +--> StrikeNova session(s)
      |
      +--> Subscription / entitlements
      |
      +--> Broker connection(s)
      |          |
      |          +--> broker-authorized credentials/tokens
      |
      +--> Trading / portfolio / analytics ownership
```

The existing Upstox OAuth/session system is a foundation to migrate and integrate, not a reason to create a second unrelated broker system.

---

# 2. Scope

## In scope

- Persistent StrikeNova user identity
- Account lifecycle
- Authentication abstraction and migration from broker-coupled authentication
- Session/device management
- Email/account identity
- Account security
- Role-based access control (RBAC)
- Permission enforcement
- Subscription plans
- Subscription lifecycle
- Feature entitlements
- Server-side entitlement enforcement
- Broker connection ownership
- Secure broker credential/token handling
- User account portal
- Admin portal
- Admin RBAC
- Audit logs
- Security events
- Operational/admin dashboards
- User support controls
- Testing, migration, rollback and documentation

## Explicitly out of scope unless separately approved

- Building a new market-data provider
- Replacing the existing broker gateway
- Rebuilding trading engines
- Introducing microservices solely for identity
- Introducing a second database solely for identity
- Building a full enterprise IAM platform
- Adding a payment provider without a separate decision
- Automatic production deployment
- Storing broker passwords or plaintext secrets in StrikeNova

---

# 3. Current-State Baseline

The application repository already contains important foundations that must be preserved:

- FastAPI backend and Next.js/React frontend.
- Upstox OAuth authentication.
- Session-based authentication and session isolation.
- Broker-neutral gateway/adapter architecture.
- User-scoped trading/execution protections in important paths.
- PostgreSQL support with local SQLite fallback.
- Existing `/settings` and broker-related UI that should be extended where appropriate.
- Security work around OAuth state, CORS, rate limiting, token/session isolation and production readiness.

Current limitation:

> Authentication is still substantially coupled to the broker OAuth/session model rather than a durable StrikeNova customer identity.

The implementation audit must be repeated against the application repository before Phase 10 coding begins. Documentation must never claim a feature is implemented merely because it is planned here.

---

# 4. Target End-State

```text
                         ┌──────────────────────────┐
                         │       StrikeNova         │
                         │      Web Application     │
                         └────────────┬─────────────┘
                                      │
                              Authentication
                                      │
                                      ▼
                         ┌──────────────────────────┐
                         │     Identity Service     │
                         │   User / Session / MFA   │
                         └────────────┬─────────────┘
                                      │
                              Authorization
                                      │
                                      ▼
                         ┌──────────────────────────┐
                         │  RBAC + Entitlements     │
                         └──────┬─────────┬─────────┘
                                │         │
                         Customer      Admin
                          Portal       Portal
                                │         │
                                └────┬────┘
                                     │
                               Application API
                                     │
             ┌───────────────────────┼────────────────────────┐
             │                       │                        │
         Trading                 Analytics               Brokers
             │                       │                        │
             └───────────────────────┼────────────────────────┘
                                     │
                                  PostgreSQL
```

---

# 5. Phase Sequence

## Phase 10.0 — Discovery, Repository Audit & Design Freeze

### Objectives

Verify the application repository before making schema or authentication changes.

### Work

- Audit current auth routers/services/middleware/dependencies.
- Audit session storage and lifecycle.
- Audit current `user_id` generation and ownership rules.
- Audit all user-owned database entities.
- Audit broker token/profile/connection handling.
- Audit frontend auth guards and routing.
- Audit `/settings`, `/brokers` and related account UI.
- Audit current database migrations.
- Audit tests covering auth, cross-user isolation and broker access.
- Identify all APIs currently deriving identity from session/broker context.
- Identify compatibility requirements for existing users/sessions.
- Define migration and rollback strategy.

### Deliverables

- Repository audit report.
- Identity dependency map.
- Current-to-target data ownership map.
- Migration inventory.
- Approved architecture specification.
- Explicit list of components that will be extended vs created.

### Exit criteria

No Phase 10 schema or implementation work starts until the actual application repository has been reconciled with this plan.

---

## Phase 10.1 — Identity Domain Foundation

### Objectives

Introduce the persistent StrikeNova account as the authoritative application identity.

### Core entities

Initial logical model:

```text
users
user_profiles
user_sessions
```

Potential fields should be finalized during design, but the model must support:

- stable internal user ID
- normalized email/identity identifier
- account status
- created/updated timestamps
- verification state
- security timestamps
- last activity metadata where justified

### Account states

At minimum:

```text
pending
active
suspended
disabled
```

Do not delete customer identity records casually. Account deletion/data retention policy requires separate privacy/legal review.

### Exit criteria

- Persistent user identity exists.
- User identity is independent from broker account.
- Existing trading ownership can resolve to the persistent user ID.
- Migration is tested and reversible.

---

## Phase 10.2 — Authentication & Account Access

### Objectives

Provide a durable StrikeNova login model while preserving broker OAuth as a broker authorization mechanism.

### Requirements

- Account registration/login flow.
- Email verification where required.
- Password reset/recovery if password authentication is selected.
- Authentication abstraction so the project can later support passkeys/OIDC without rewriting application authorization.
- Secure session issuance.
- Secure cookie/session strategy.
- Session expiration and revocation.
- Rate limiting and abuse protection.
- Authentication event logging.

### Important rule

Broker OAuth must no longer be the only definition of whether a person has a StrikeNova account.

### Exit criteria

A user can authenticate to StrikeNova without requiring a broker connection, subject to the selected account policy.

---

## Phase 10.3 — Session & Device Management

### Objectives

Replace purely ephemeral identity assumptions with durable, revocable session management.

### User capabilities

- View active sessions/devices.
- See approximate device/session metadata where appropriate.
- Revoke individual session.
- Revoke all other sessions.
- Automatic expiry.
- Security notification/event when relevant.

### Security requirements

- Session identifiers must be high entropy.
- Tokens must not be logged.
- Session ownership must be user-scoped.
- Session revocation must be enforced server-side.
- Avoid storing unnecessary device/IP data.

### Exit criteria

Cross-user session access tests pass and revoked sessions cannot access protected APIs.

---

## Phase 10.4 — Authorization & RBAC

### Objectives

Separate authentication from authorization and establish explicit roles/permissions.

### Initial roles

```text
SUPER_ADMIN
SUPPORT_ADMIN
OPERATIONS_ADMIN
FINANCE_ADMIN
CUSTOMER
```

### Permission domains

```text
users.read
users.manage
users.suspend
sessions.revoke
subscriptions.read
subscriptions.manage
entitlements.manage
brokers.read
brokers.manage
operations.read
audit.read
admin.manage
```

Exact permissions must be finalized during design.

### Rules

- Backend is authoritative.
- Frontend visibility is not security.
- Every protected admin endpoint checks permission.
- Customer APIs must enforce ownership.
- No role may access secrets merely because it can view a broker connection.
- Super-admin capability must be rare and auditable.

### Exit criteria

Authorization matrix is implemented and tested for allow/deny behavior and privilege escalation.

---

## Phase 10.5 — Subscription & Entitlement Foundation

### Objectives

Create the server-authoritative mechanism that determines what a user can use.

### Logical entities

```text
subscription_plans
subscriptions
entitlements
```

Potential future entity:

```text
subscription_events / payment_transactions
```

### Initial commercial plans

The current product concept includes:

- Monthly: ₹500
- Annual: ₹4,000

These are product assumptions and must not be hard-coded into business logic.

### Subscription states

At minimum:

```text
trial
active
past_due
cancelled
expired
suspended
```

### Entitlement principle

```text
User
 ↓
Subscription
 ↓
Entitlements
 ↓
Server-side authorization
 ↓
Feature/API access
```

The frontend may hide unavailable features, but the backend must enforce the same rule.

### Exit criteria

A user cannot bypass plan restrictions by directly calling APIs.

---

## Phase 10.6 — Broker Connection Ownership & Credential Security

### Objectives

Refactor broker ownership from session-centric to user-centric without breaking the existing broker gateway.

### Target model

```text
user
  ↓
broker_connection
  ↓
broker/provider metadata
  ↓
secure credential/token reference
```

### Requirements

- One or more broker connections can belong to a user where supported.
- Broker connection has explicit owner user ID.
- OAuth authorization is linked to the correct user.
- Tokens/credentials are encrypted or securely referenced according to the chosen implementation.
- No plaintext secrets in database logs, source code or admin UI.
- Admin cannot view raw broker secrets.
- User can connect/disconnect broker.
- User can revoke broker authorization where supported.
- Broker failures affect only the relevant user/connection.

### Exit criteria

- Cross-user broker access tests pass.
- Session changes do not change broker ownership.
- Broker connection survives normal user re-login.
- Existing broker gateway remains reusable.

---

## Phase 10.7 — User Account Portal

### Objective

Turn the existing account/settings area into a complete customer self-service portal.

### Target navigation

```text
My Account
├── Profile
├── Security
├── Sessions
├── Subscription
├── Broker Connections
├── Preferences
└── Privacy & Data
```

### Features

#### Profile
- Name/display information as needed.
- Email identity.
- Verification state.
- Account status.

#### Security
- Change authentication credentials where applicable.
- MFA/passkey management when implemented.
- Security events.

#### Sessions
- Active devices/sessions.
- Revoke.

#### Subscription
- Current plan.
- Status.
- Renewal/expiry.
- Entitlements.

#### Broker Connections
- Connected broker.
- Connection status.
- Last successful authorization/connection.
- Connect/disconnect/reconnect.

#### Privacy & Data
- Privacy controls.
- Account/data export/delete workflows when legally and technically appropriate.

### Exit criteria

A customer can manage the majority of normal account operations without administrator intervention.

---

## Phase 10.8 — Admin Portal Foundation

### Objective

Create a protected `/admin` experience using the same authoritative backend identity/authorization system.

### Main areas

```text
/admin
├── Dashboard
├── Users
├── Subscriptions
├── Entitlements
├── Broker Connections
├── Security / Audit
├── Operations
└── Settings
```

### Admin dashboard

Initial metrics:

- Registered users.
- Active users.
- New users.
- Active subscriptions.
- Expiring subscriptions.
- Suspended users.
- Broker connection health.
- Authentication/security events.
- API/system health.

Metrics must be defined precisely and must not claim financial/revenue figures unless backed by actual payment data.

---

## Phase 10.9 — Admin User Management

### Requirements

Search/filter users by:

- User ID.
- Email/identity.
- Status.
- Plan.
- Broker connection state.
- Created date.
- Last activity.

### User detail

```text
Account
Security
Sessions
Subscription
Entitlements
Broker Connections
Activity
Audit History
```

### Admin actions

- Suspend account.
- Reactivate account.
- Revoke sessions.
- View subscription state.
- Grant/revoke controlled entitlements.
- Extend access where policy permits.
- Disconnect/revoke broker authorization where operationally necessary and supported.

### Safety

Destructive/high-impact actions require confirmation and audit logging.

Admin must never receive raw passwords, OAuth authorization codes or broker secrets.

---

## Phase 10.10 — Subscription & Entitlement Administration

### Requirements

- Create/manage plans.
- Activate/deactivate plans.
- Assign plan.
- Extend subscription.
- Cancel/suspend subscription.
- Grant controlled promotional access.
- Override entitlement only through explicit audited operations.
- View subscription history.

### Rule

Manual admin overrides must be represented as explicit state/events rather than silently mutating history.

---

## Phase 10.11 — Audit & Security Event System

### Objective

Create a durable audit trail for security-sensitive and administrative operations.

### Event categories

```text
AUTH_LOGIN
AUTH_LOGOUT
AUTH_FAILED
SESSION_CREATED
SESSION_REVOKED
USER_CREATED
USER_SUSPENDED
USER_REACTIVATED
ROLE_CHANGED
SUBSCRIPTION_CHANGED
ENTITLEMENT_CHANGED
BROKER_CONNECTED
BROKER_DISCONNECTED
ADMIN_LOGIN
ADMIN_ACTION
SECURITY_EVENT
```

### Minimum audit fields

- event ID
- actor user ID where applicable
- target user ID where applicable
- event/action type
- timestamp
- result
- safe metadata/context

IP/device information should only be retained where justified by security, operational or legal requirements.

### Rules

- Audit logs are append-oriented.
- Sensitive secrets never appear in audit metadata.
- Admin actions must be attributable.
- Logs must have retention rules.
- Audit access itself should be permission-controlled.

---

## Phase 10.12 — Admin MFA & High-Risk Controls

### Requirements

- Mandatory MFA for privileged administrators.
- Re-authentication/step-up authentication for high-risk actions where justified.
- Strong session expiry for admin sessions.
- Privilege checks on every admin API.
- Protection against privilege escalation.
- Security event generation for admin authentication and sensitive changes.

### High-risk actions

Examples:

- Changing admin roles.
- Suspending/reactivating accounts.
- Changing entitlements.
- Disconnecting broker authorization.
- Changing security configuration.

---

## Phase 10.13 — Notifications & Account Lifecycle Automation

### Initial capabilities

- Account verification notifications.
- Password/security notifications where applicable.
- Subscription expiry reminders.
- Broker authorization status notifications.
- Security alerts for important account events.

### Background jobs

Only add scheduled/background infrastructure where the requirement is real.

Examples:

```text
subscription expiry checks
session cleanup
security-event processing
notification delivery
```

Prefer existing infrastructure patterns before introducing new infrastructure.

---

## Phase 10.14 — Observability & Operations

### Requirements

Admin/operations should be able to determine:

- Is authentication working?
- Are sessions being created/revoked?
- Are broker connections healthy?
- Are authorization failures increasing?
- Are subscription checks working?
- Are background jobs failing?
- Is PostgreSQL healthy?

### Principle

Do not expose sensitive internals or secrets merely for observability.

Operational metrics should be measurable and documented.

---

## Phase 10.15 — Migration, Compatibility & Rollback

### Objective

Move existing development/test users and broker sessions to the new identity model without breaking the application.

### Requirements

- Migration script.
- Database backup/recovery procedure.
- Data mapping.
- Idempotent migration where practical.
- Compatibility window for old sessions if required.
- Explicit cutover.
- Rollback plan.
- Post-migration validation.

### Migration rule

Never destroy the existing authentication path before the new identity path has passed integration and isolation testing.

---

## Phase 10.16 — Comprehensive Testing & Security Validation

### Unit tests

- User creation.
- User state transitions.
- Permission evaluation.
- Entitlement evaluation.
- Subscription transitions.
- Session lifecycle.
- Audit event creation.

### Integration tests

- Login.
- Logout.
- Session revocation.
- Broker connection ownership.
- User-scoped APIs.
- Subscription enforcement.
- Admin actions.
- Audit logging.

### Security tests

- Cross-user data access.
- Cross-user broker access.
- Session fixation.
- Session theft/replay handling where applicable.
- Privilege escalation.
- Unauthorized admin API calls.
- Entitlement bypass.
- IDOR-style ownership failures.
- Rate-limit behavior.
- CSRF/state protection where applicable.
- Secret leakage checks.

### Regression tests

Existing trading, paper-trading, analytics, GEX, market-data and broker tests must remain green.

### Exit criteria

No known critical/high-severity authorization or credential-isolation issue remains open.

---

## Phase 10.17 — Production Readiness Gate

Before commercial launch:

- Authentication verified.
- Authorization verified.
- User isolation verified.
- Broker credential isolation verified.
- Admin MFA verified.
- Auditability verified.
- Subscription enforcement verified.
- Database migrations verified.
- Backup/recovery procedure verified.
- Monitoring/alerts verified.
- Rate limiting verified.
- Privacy/security documentation reviewed.
- Manual production deployment procedure documented.
- Rollback procedure documented.

This phase does **not** authorize production deployment. Deployment remains separately controlled by the project owner.

---

# 6. Target Data Model

The following is a logical target, not a final migration schema. Exact columns, constraints and indexes must be derived from the current database implementation.

```text
users
 ├── user_profiles
 ├── user_sessions
 ├── user_roles ─── roles ─── permissions
 ├── subscriptions ─── subscription_plans
 ├── entitlements
 ├── broker_connections
 └── audit_logs
```

### Important constraints

- Stable primary keys.
- Foreign-key ownership where appropriate.
- Unique normalized account identity.
- Explicit account status.
- Explicit timestamps.
- Index user-owned high-volume access paths.
- Do not store secrets unnecessarily.
- Encrypt/protect sensitive credentials.
- Design migrations for PostgreSQL production and SQLite development compatibility where required by the existing project.

---

# 7. API Surface — Target Categories

Exact routes must be reconciled with existing routers before implementation.

## Customer

```text
GET    /api/account
PATCH  /api/account
GET    /api/account/security
GET    /api/account/sessions
DELETE /api/account/sessions/{id}
POST   /api/account/sessions/revoke-all
GET    /api/account/subscription
GET    /api/account/entitlements
GET    /api/account/brokers
POST   /api/account/brokers/{broker}/connect
POST   /api/account/brokers/{id}/disconnect
```

## Admin

```text
GET    /api/admin/dashboard
GET    /api/admin/users
GET    /api/admin/users/{id}
POST   /api/admin/users/{id}/suspend
POST   /api/admin/users/{id}/reactivate
POST   /api/admin/users/{id}/revoke-sessions
GET    /api/admin/subscriptions
POST   /api/admin/subscriptions/{id}/extend
GET    /api/admin/entitlements
POST   /api/admin/entitlements/override
GET    /api/admin/brokers
GET    /api/admin/audit
GET    /api/admin/security/events
```

These are examples for architectural planning, **not instructions to create duplicate endpoints**. Existing API contracts must be reused/extended first.

---

# 8. Security Model

## Authentication

Authentication proves identity.

## Authorization

Authorization determines what that identity may do.

## Ownership

Every customer-owned resource must resolve to the authenticated user and enforce ownership server-side.

## Entitlements

Subscription/feature access must be evaluated server-side.

## Secrets

Broker credentials/tokens are security-sensitive and must never be exposed through ordinary account/admin APIs.

## Admin

Privileged actions require stronger controls and complete auditability.

---

# 9. Performance & Scalability

Initial target:

- Support at least the planned 1,000-user commercial deployment without premature infrastructure complexity.
- Keep PostgreSQL as the authoritative identity store.
- Avoid adding Redis/microservices solely because they may be useful later.
- Add distributed session/rate-limit infrastructure only when measured requirements justify it.
- Index common account/ownership/authorization queries.
- Avoid loading large audit histories into normal dashboard requests.
- Paginate users, audit events and subscription histories.

Future scale target:

- 10,000+ users.

Architecture should permit scaling without requiring an identity rewrite.

---

# 10. UX Principles

- Account functions should be easy to find.
- Security-sensitive actions should be clearly explained.
- Do not expose technical secrets or confusing broker internals.
- Subscription status should be obvious.
- Users should understand what features they have access to.
- Admin actions should show impact before confirmation.
- Error messages should help users without leaking security-sensitive information.
- Mobile/responsive behavior must be considered for account/admin screens.

---

# 11. Definition of Done

Phase 10 is complete only when all of the following are true:

1. StrikeNova has a persistent application identity independent of broker identity.
2. Users can authenticate and manage their account securely.
3. Sessions are persistent/revocable and user-scoped.
4. Authorization is explicit and server-enforced.
5. Customer data cannot cross user boundaries.
6. Broker connections are owned by users rather than transient sessions.
7. Broker secrets are protected and never exposed to admins/users unnecessarily.
8. Subscription state is persistent and authoritative.
9. Entitlements are server-enforced.
10. Customer account portal is operational.
11. Admin portal is operational.
12. Admin RBAC is enforced.
13. Privileged admin access requires MFA.
14. Sensitive administrative operations are auditable.
15. Migration and rollback have been tested.
16. Unit/integration/security/regression tests pass.
17. Production-readiness gates are documented and passed.
18. Project memory and application documentation accurately reflect implementation state.
19. No production deployment occurs without explicit owner approval.

---

# 12. Implementation Order Summary

```text
10.0 Repository Audit / Design
        ↓
10.1 Persistent Identity
        ↓
10.2 Authentication
        ↓
10.3 Sessions / Devices
        ↓
10.4 RBAC / Authorization
        ↓
10.5 Subscription / Entitlements
        ↓
10.6 Broker Ownership Migration
        ↓
10.7 User Portal
        ↓
10.8 Admin Portal Foundation
        ↓
10.9 Admin User Management
        ↓
10.10 Subscription Administration
        ↓
10.11 Audit / Security Events
        ↓
10.12 Admin MFA / High-Risk Controls
        ↓
10.13 Notifications / Lifecycle Jobs
        ↓
10.14 Observability / Operations
        ↓
10.15 Migration / Rollback
        ↓
10.16 Full Testing / Security Validation
        ↓
10.17 Production Readiness Gate
```

---

# 13. Dependencies

Phase 10 depends on:

- Current PostgreSQL/database foundation.
- Existing authentication/session code.
- Existing broker gateway/adapter architecture.
- Existing user-scoped trading/execution protections.
- Existing frontend routing/layout.
- Existing testing/CI framework.

Phase 10 should **not** block all analytics/research work, but production commercial onboarding should be blocked until the relevant identity, security and entitlement gates are complete.

---

# 14. Risks

| Risk | Severity | Mitigation |
|---|---|---|
| Broker auth remains coupled to user identity | High | Introduce persistent StrikeNova identity first |
| Cross-user data leakage | Critical | Ownership checks + isolation tests |
| Admin privilege escalation | Critical | RBAC + backend enforcement + security tests |
| Broker token exposure | Critical | Secure storage + secret redaction + no admin plaintext access |
| Subscription bypass | High | Server-side entitlement checks |
| Migration breaks existing sessions | High | Compatibility window + rollback |
| Over-engineering IAM | Medium | Start with modular monolith and explicit boundaries |
| Premature paid infrastructure | Medium | Reuse PostgreSQL/current infrastructure until measured need |
| Audit logs leak secrets | High | Strict event schema + redaction tests |
| Admin becomes a second business-logic system | High | Shared backend/domain services |

---

# 15. Documentation Updates Required During Implementation

When each sub-phase is completed, update:

- `PROJECT_STATUS.md`
- `04_CURRENT_STATE.md`
- `05_DECISIONS.md` for accepted architectural decisions
- `02_ARCHITECTURE.md` when target architecture becomes implemented
- `07_TECHNICAL_DEBT.md` when debt is created/resolved
- Phase-specific implementation notes as needed

Never mark a phase complete from discussion alone. Completion requires repository evidence and tests.

---

# 16. Status Convention

Use:

- `🔴 Not Started`
- `🟡 In Progress`
- `🟢 Complete`
- `🟠 Blocked`
- `⚪ Deferred`

A phase may only become `🟢 Complete` after its documented exit criteria are satisfied.
