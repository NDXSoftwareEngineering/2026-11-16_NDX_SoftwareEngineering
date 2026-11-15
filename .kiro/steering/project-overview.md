---
inclusion: always
---

# Project Overview: Software Engineering Practice with RealWorld Conduit

Baseline version: 1.3 (2026-11-16). Status: APPROVED by the user. v1.0 approved 2026-11-16 (DEC-001); decisions DEC-002 to DEC-008 applied in v1.2; v1.1 NASA and SWEBOK references and NASA applicability approved in v1.3 (DEC-009). Decisions: `docs/00-governance/decision-log.md`.
Companion steering files: `engineering-process.md`, `requirements-and-sources.md`, `technology-stack.md`, `quality-assurance.md`. If these files disagree, stop and report the conflict; do not pick one silently.

## 1. Purpose

Practice Software Engineering by building a complete RealWorld "Conduit" application (a Medium.com-style blogging platform) through a controlled, five-phase lifecycle with documented evidence and quality gates.

The product is the learning vehicle. The engineering process, its artifacts, and its evidence are the primary learning outcomes.

## 2. Learning Objectives

The project is successful as a learning exercise when the user can show, with evidence, that they:

1. Derived traceable requirements from a public specification without inventing product scope.
2. Produced a design that satisfies those requirements and records its decisions and trade-offs.
3. Built backend, frontend, and database components that implement the approved design.
4. Planned and executed verification at unit, integration, API, and end-to-end levels, with recorded results.
5. Deployed and operationally checked the application in a documented environment.
6. Applied NASA lifecycle and assurance practices (NPR 7150.2D, NASA-STD-8739.8B, NASA Software Engineering Handbook) and SWEBOK V4 knowledge, scaled to a learning project.

## 3. Product Scope

The product scope is defined only by the RealWorld sources listed in `requirements-and-sources.md`. At the summary level, the RealWorld documentation (`docs/.../implementation-creation/features.md`) lists:

- JWT user authentication (login, sign-up, logout on the settings page).
- Create, read, and update users (no deletion).
- Create, read, update, and delete articles.
- Create, read, and delete comments on articles (no update).
- Paginated article lists.
- Favoriting articles.
- Following users.

This list is a scope summary, not the requirements baseline. Detailed requirements are produced and approved in Phase 1.

## 4. Project Boundaries

In scope:
- One backend that conforms to the RealWorld API contract (`specs/api/openapi.yml` and the Hurl suite).
- One React frontend that conforms to the RealWorld UI templates, routing, shared theme, and E2E selector contract (`specs/e2e/SELECTORS.md`).
- One PostgreSQL 17 database.
- Lifecycle documentation, test evidence, and a deployment to a documented target.

Out of scope unless the user approves a change:
- Features not defined by RealWorld sources (for example: admin panels, notifications, search, image upload, account deletion).
- Mobile applications.
- Changes to the RealWorld specification or its test suites. `realworld-main/` is a read-only reference.
- Docker, WSL2, Kubernetes, cloud infrastructure, message queues, and caches (deferred or excluded; see `technology-stack.md`).
- Reuse of any files from the previous OneDrive project folder.

## 5. Stakeholders and Responsibilities

| Stakeholder | Responsibility |
| --- | --- |
| User (project owner, learner) | Approves scope, requirements, design, technology changes, and every quality gate. Acts as product owner and acceptance authority. |
| Kiro (AI engineering agent) | Performs analysis, documentation, design, construction, and testing as directed. Reports evidence, assumptions, and open issues. Does not approve its own work. |
| RealWorld maintainers (external) | Own the product specification and shared test suites. Not available for clarification; ambiguities are recorded and resolved by user decision. |
| NASA / IEEE Computer Society (external) | Authors of the methodology references. Their documents guide process only. |

Independence note: Kiro reviewing its own work is a self-check. It is not independent verification and validation (IV&V). See `quality-assurance.md`.

## 6. Agreed Technology Stack

| Area | Technology |
| --- | --- |
| Operating system | Windows 11 |
| IDE | Kiro |
| Backend | Python 3.12, Django, Django Ninja |
| Frontend | React, Vite |
| Database | PostgreSQL 17 |
| Backend testing | pytest |
| Frontend / E2E testing | Playwright |
| Version control | Git, GitHub |

Approved additions: Vitest (frontend unit tests, DEC-003); Hurl for Windows (API conformance, DEC-004); Ruff, ESLint, Prettier, pip-audit, `venv` + `pip` (DEC-005). Decisions are recorded in `docs/00-governance/decision-log.md`.

Initial development runs directly on Windows. Docker and WSL2 are deferred. Details and change rules are in `technology-stack.md`.

## 7. Lifecycle Phases

Phases run strictly in order. Each ends with a quality gate that the user must explicitly accept (details in `engineering-process.md`).

1. Requirements Engineering (QG1)
2. Software Design (QG2)
3. Software Construction (QG3)
4. Software Testing (QG4)
5. Deployment (QG5)

## 8. Key Deliverables

| Phase | Key deliverables |
| --- | --- |
| Pre-phase: Governance (not one of the five phases) | These steering files; source inventory; known issues list (`requirements-and-sources.md` §8) |
| 1. Requirements | Software Requirements Specification (SRS); use cases; requirements traceability matrix (RTM) v1; issues/assumptions log |
| 2. Design | Software Design Description (architecture, components, data, API, UI); decision records; RTM v2; test strategy outline |
| 3. Construction | Source code in Git; unit tests; build and run instructions; static analysis results; RTM v3 |
| 4. Testing | Test plan; test cases; test execution reports (pytest, Hurl, Playwright); defect log; RTM v4 with verification status |
| 5. Deployment | Deployment guide; environment configuration description; smoke test report; release notes; final project report |

## 9. Constraints

- C-01: Product requirements come only from RealWorld sources.
- C-02: The agreed technology stack may change only with explicit user approval.
- C-03: Development platform is Windows 11, without Docker or WSL2 for initial development.
- C-04: Phases are sequential; no implementation before Phase 2 is accepted.
- C-05: Files under `realworld-main/specs/` are the conformance reference and must not be edited to make tests pass.
- C-06: No fabricated results. Every PASS needs evidence.
- C-07: The repository has a README that explains how to run the application locally (DEC-007, from RealWorld `expectations.md`).

## 10. Assumptions

Each assumption must be confirmed or replaced as the project progresses.

- A-01: CONFIRMED. The RealWorld baseline is upstream commit `25588846` (2026-11-02), pinned as the `realworld-main/` submodule. The previously downloaded copy was identical (`docs/00-governance/realworld-baseline.md`).
- A-02: The user is the single approver for all gates and changes.
- A-03: The new workspace `E:\2026-11-16-SoftwareEngineering` replaces the OneDrive project folder named in the 2026-11-15 note.
- A-04: The deployment target is not yet defined. It is decided in the Design phase and confirmed before Phase 5.
- A-05: The project is a single-developer learning project with no safety-critical software and no NASA project category. NASA practices are applied proportionately. Any NPR 7150.2D Appendix D software class (the closest are Class E and Class F) is assigned only as a learning exercise, with user approval (see `engineering-process.md` §11).
- A-06: CONFIRMED by the user. The Git repository root (monorepo, DEC-006) is `E:\2026-11-16-SoftwareEngineering`.
