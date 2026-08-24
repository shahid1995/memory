# StrikeNova — Decision Log

This is the authoritative record of important architectural/product decisions. New significant decisions should be appended rather than silently replacing historical decisions.

## DECISION-001 — Use a dedicated project-memory repository

**Status:** Accepted

**Decision:** Maintain StrikeNova's long-term architecture, research, roadmap, decisions and status in `shahid1995/memory`.

**Reason:** Long conversations should not be the only source of project memory. Documentation must survive conversation boundaries and provide a stable source of truth.

**Consequence:** Important decisions made in the Project Control Center should be promoted into this repository.

## DECISION-002 — This conversation is the StrikeNova Project Control Center

**Status:** Accepted

**Decision:** Use this conversation for ongoing feature discussions, research, architecture decisions, planning and coordination.

**Reason:** The user wants one central place to discuss new features while keeping permanent project memory in GitHub.

**Consequence:** The assistant should use the memory repository to reconstruct context and should identify documentation updates after important decisions.

## DECISION-003 — Prefer genuinely free/lifetime-free tooling

**Status:** Accepted

**Decision:** Prefer genuinely free tools, services and data sources where practical. Do not assume paid market-data services.

**Reason:** Keeping the platform accessible and minimizing recurring costs is a standing project constraint.

**Consequence:** Any paid dependency should require explicit consideration/approval.

## DECISION-004 — Separate project memory from application code

**Status:** Accepted

**Decision:** The `memory` repository contains documentation and project knowledge; the StrikeNova application repository contains the implementation.

**Reason:** This creates a clear distinction between what the system should be and what has actually been implemented.

**Consequence:** Architecture documentation must be reconciled against the application repository.

## DECISION-005 — Manual production deployment control

**Status:** Accepted

**Decision:** Development tooling should not automatically deploy production changes unless explicitly approved by the project owner.

**Reason:** The user wants control over production deployment.

**Consequence:** Implementation and deployment are separate workflow stages.

## DECISION-006 — Research must precede unvalidated trading features

**Status:** Accepted

**Decision:** New trading indicators/strategies should be treated as research hypotheses until validated.

**Reason:** Trading analytics can appear persuasive without demonstrating predictive robustness.

**Consequence:** Production features should document formulas, data requirements, testing and limitations.

## Decision Template

### DECISION-XXX — Title

**Status:** Proposed / Accepted / Superseded / Rejected

**Decision:**

**Reason:**

**Alternatives considered:**

**Trade-offs:**

**Consequences:**

**Date:**

**Related documents:**
