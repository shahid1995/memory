# StrikeNova Project Memory

This repository is the long-term memory and documentation system for **StrikeNova**.

## How this repository is used

The **StrikeNova Project Control Center** conversation is the primary working discussion space. The repository stores the durable project knowledge that should survive long conversations and new sessions.

### Core documents

- `00_MASTER_CONTEXT.md` — compact project context and long-term operating picture.
- `01_PROJECT_CONSTITUTION.md` — permanent principles and constraints.
- `02_ARCHITECTURE.md` — current architectural model and boundaries.
- `03_ROADMAP.md` — product and engineering roadmap.
- `04_CURRENT_STATE.md` — current verified/known project state.
- `05_DECISIONS.md` — important architectural/product decisions.
- `06_KNOWN_ISSUES.md` — unresolved questions, risks and verification items.
- `07_TECHNICAL_DEBT.md` — known technical/documentation debt.

Additional directories such as `research/`, `phases/`, `product/`, `infrastructure/` and `security/` should be created only when the project needs them.

## Operating rule

Do not rely on a long chat history as the only project memory. Important decisions and durable knowledge should be promoted into this repository.

When adding a significant feature:

1. Discuss it in the Project Control Center.
2. Audit the existing architecture.
3. Research where necessary.
4. Decide the approach.
5. Update the relevant memory documents.
6. Implement in the actual StrikeNova application repository.
7. Test and validate.
8. Update `04_CURRENT_STATE.md` and related records.

## Security

Never store credentials, API keys, access tokens, passwords, production secrets or private customer information in this repository.
