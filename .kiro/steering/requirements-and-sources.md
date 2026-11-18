---
inclusion: always
---

# Requirements and Sources

Baseline version: 1.4 (2026-11-16). Status: APPROVED by the user on 2026-11-16 with the acceptance of Quality Gate S1 (evidence E-09). After that approval only status markers were updated to record E-09. History and evidence: `docs/00-governance/decision-log.md` §1 and §4.

## 1. Source Hierarchy

### Group A: Product requirements (define WHAT Conduit must do)

All paths are relative to `realworld-main/` (Git submodule pinned to upstream commit `25588846`, see `docs/00-governance/realworld-baseline.md`) unless stated otherwise. Precedence is highest first. RealWorld's backend documentation states that where prose and tests disagree, the tests win (`docs/src/content/docs/specifications/backend/introduction.md`).

| ID | Source | Location | Role |
| --- | --- | --- | --- |
| SRC-API | OpenAPI 3.1 contract, API version 2.0.0 | `specs/api/openapi.yml` | Authoritative API contract |
| SRC-HURL | Hurl API test suite (source of truth for API tests) | `specs/api/hurl/*.hurl` | Authoritative API behaviour, including error cases and pagination |
| SRC-E2E | Playwright E2E suite and selector contract | `specs/e2e/*.spec.ts`, `specs/e2e/SELECTORS.md`, `specs/e2e/playwright.base.ts` | Authoritative frontend behaviour and UI contract |
| SRC-DOC | RealWorld documentation site source | `docs/src/content/docs/**` (specifications, implementation-creation) | Prose description of features, endpoints, response format, errors, CORS, routing, templates, styles |
| SRC-THEME | Shared CSS theme | `assets/theme/styles.css` | UI styling reference |
| SRC-DOCX | Exported documentation (not in the upstream repository; local only, not committed) | `_archive/realworld-main-download-2026-11-16/RealWorld apps Documentation.docx` (workspace path) | Lowest precedence; older snapshot (see ISS-003) |
| SRC-BRUNO | Bruno collection | `specs/api/bruno/` | Generated from Hurl; not an independent source |

Rules:
- When Group A sources conflict, apply the precedence above, record the conflict as an issue, and ask the user to confirm the resolution.
- Do not edit anything under `realworld-main/`. It is a read-only conformance reference.
- Statements about the public demo API (`api.realworld.show`), CodebaseShow, or GitHub processes describe the RealWorld ecosystem, not Conduit requirements, unless the user decides otherwise.

### Group B: Methodology (define HOW the work is done)

| ID | Source | Local file | Role |
| --- | --- | --- | --- |
| MTH-NPR | NASA NPR 7150.2D, NASA Software Engineering Requirements. Effective 2022-03-08, expires 2027-03-08. 89 pages. | `The NASA Software Engineering Handbook/The NASA Software Engineering Requirements, NPR 7150.2D.pdf` | Lifecycle activities and engineering practice (SWE-numbered requirements in Chapters 3–5; classification in Appendix D) |
| MTH-STD | NASA-STD-8739.8B, Software Assurance and Software Safety. Approved 2022-09-08. 70 pages. | `The NASA Software Engineering Handbook/The NASA Software Assurance and Software Safety Standard requirements, NASA-STD-8739.8B.pdf` | Software assurance tasks mapped to SWE numbers; IV&V and independence definitions |
| MTH-HB-LOCAL | Local partial export of the NASA Software Engineering and Software Assurance Handbook, NASA-HDBK-2203. Book A title material shows "NASA-HDBK-2203B, Approved: March 08, 2022"; Book A pages also show website posts dated October 2025. 12 files, 151 pages: Books A–E (section listings of SWE titles and requirement statements; Book D is a topic listing; Book E is one page) and the full guidance pages of SWE-002 (two files with identical extracted page text but different file hashes), SWE-003, SWE-004, SWE-005, SWE-006, SWE-013. | `The NASA Software Engineering Handbook/` (all files except the NPR and STD PDFs) | Guidance and lessons learned for the listed SWEs only. Not a complete Handbook. |
| MTH-HB-ONLINE | Official online NASA Software Engineering Handbook (space SWEHBVD, called "Version D" in the 2026-11-15 note) | https://swehb.nasa.gov/spaces/SWEHBVD/overview | Complete guidance. NOT CHECKED: not fetched or read during governance setup. |
| MTH-SWEBOK | SWEBOK Guide v4.0a (released August 2026, IEEE Computer Society), 411 pages, 18 knowledge areas | `swebok-v4.pdf` (local only, not committed) | Professional knowledge reference |

The text of all these PDFs was extracted with pypdf on 2026-11-16. SWE numbers cited in these steering files were checked against the extracted NPR 7150.2D text. Exactly what was read in each source is recorded in `docs/00-governance/source-inventory.md` §3.

Handbook coverage rule: this baseline does not cite Handbook guidance content. Before citing guidance from the Handbook for any SWE, read that SWE's page in MTH-HB-ONLINE, or in MTH-HB-LOCAL if the SWE is one of the six exported, and record the page and version label. Do not claim Handbook coverage for content that was not read.

Rules:
- NASA and SWEBOK content never becomes a Conduit product requirement automatically. A process practice may be adopted as a project engineering constraint (CON) only with user approval.
- When citing a NASA requirement (for example an SWE number), cite it only after reading it in the source. If the source was not read, say so.
- Most NPR 7150.2D requirements are addressed to NASA roles (OCE, Center Director, project manager). In this project they are learning references: the user holds the project manager role and Kiro acts as the developer. Institutional requirements (Chapter 2, e.g. SWE-002 to SWE-006) do not apply to the project.
- Using a NASA practice is a tailoring choice in the spirit of SWE-121 (document approved tailoring in plans) and SWE-125 (requirements mapping matrix). It does not claim compliance with NPR 7150.2D.

### Unavailable or unverified sources

If a source is missing, unreadable, or was not actually read, report it. Never describe the content of a source that was not inspected.

## 2. Requirements Classification

| Type | Prefix | Definition |
| --- | --- | --- |
| Functional requirement | FR | A behaviour the system must provide (for example: register a user). |
| Non-functional requirement | NFR | A quality attribute with a measurable criterion (security, performance, usability, maintainability, compatibility). |
| Business rule | BR | A domain policy or invariant (for example: only the author may delete an article). |
| Interface requirement | IR | A contract with an external party: HTTP API, UI selectors, routes, storage keys, data formats. |
| Constraint | CON | A restriction on the solution (technology, platform, process). Engineering constraints come from user decisions, not RealWorld. |
| Proposed decision | PROP | A requirement-level choice the agent proposes to fill a gap. Not binding until the user approves; then it becomes DEC-backed. |

Functional areas used in IDs (Phase 1 may refine this list with user approval): AUTH (authentication and users), PROF (profiles and following), ART (articles), FEED (lists, feed, pagination), FAV (favorites), CMT (comments), TAG (tags), ERR (error handling), UI (frontend pages and navigation), SEC (security), GEN (general).

## 3. Identification and Numbering

- Format: `<TYPE>-<AREA>-<NNN>`, for example `FR-ART-003`, `IR-UI-012`, `NFR-SEC-002`. CON and PROP may omit the area: `CON-004`.
- IDs are never reused or renumbered. Deleted requirements stay in the SRS with status DELETED.
- Each requirement record contains: ID, title, statement ("The system shall ..."), type, source reference (source ID plus file and section, operation ID, or test name), rationale, priority (MUST / SHOULD / COULD), verification method (Test / Analysis / Inspection / Demonstration), acceptance criterion, status (DRAFT / APPROVED / CHANGED / DELETED), and related issue IDs.
- A requirement with no source must be typed CON or PROP and say so in its source field.
- Other IDs: issues `ISS-NNN`, assumptions `A-NN`, decisions `DEC-NNN`, change requests `CR-NNN`, defects `DEF-NNN`, test cases `TC-<AREA>-NNN`, use cases `UC-NN`.

## 4. Quality Attributes of a Requirement

Each requirement must be: necessary, unambiguous, verifiable, consistent with other requirements, feasible on the agreed stack, singular (one requirement per statement), and traceable to a source. Vague words ("fast", "secure", "easy", "appropriate") require a measurable criterion or are rejected.

## 5. Requirements Traceability Matrix (RTM)

One RTM, extended phase by phase. Bidirectional: every row traces forward to verification, and every design element, code module, and test traces back to a requirement or approved decision.

| Column | Filled in |
| --- | --- |
| Requirement ID, type, short title | Phase 1 |
| Source reference(s) | Phase 1 |
| Use case(s) | Phase 1 |
| Design element(s) | Phase 2 |
| Code module(s) | Phase 3 |
| Test case(s) and suite (pytest / Hurl / Playwright / manual) | Phases 3–4 |
| Verification status (PASS / FAIL / NOT RUN / BLOCKED / N/A) and evidence link | Phase 4 |

Coverage checks at each gate:
- Forward: every APPROVED requirement has entries for the current phase.
- Backward: no design element, module, or test without a requirement or DEC (orphans are reported).
- Source coverage: every OpenAPI operation, Hurl file, and E2E spec file maps to at least one requirement.

## 6. Change Control

Requirements are baselined at QG1. After that:

1. Raise a change request `CR-NNN`: description, reason, source, affected requirements, impact on design/code/tests/schedule.
2. Agent performs impact analysis using the RTM.
3. User approves or rejects. No change is implemented before approval.
4. On approval: update the SRS (status CHANGED, history kept), RTM, and affected artifacts; re-run affected reviews and tests.

The same procedure applies to approved design decisions and the technology stack.

## 7. Ambiguity, Conflict, and Gap Management

When a source is missing, ambiguous, conflicting, or unverifiable:

1. Record an issue `ISS-NNN` with: type (MISSING / AMBIGUOUS / CONFLICT / UNVERIFIABLE), exact source references and quotations, impact, options, and a recommendation.
2. Do not resolve it silently. Apply the source precedence in §1 as a recommendation only.
3. The user decides. The decision is recorded as a DEC and referenced from the affected requirement.
4. Unresolved issues may be carried past a gate only with explicit user acceptance recorded in the gate record.

## 8. Known Issues at Baseline (2026-11-16)

| ID | Type | Description | Proposed handling |
| --- | --- | --- | --- |
| ISS-001 | MISSING | The downloaded `realworld-main/` had no Git metadata; the upstream commit was unknown. | RESOLVED as a fact: all 337 files of the inspected copy are identical to upstream commit `25588846` (`docs/00-governance/realworld-baseline.md`). The submodule is approved (DEC-006 b); pinning to this commit is APPROVED (DEC-012, E-09). |
| ISS-002 | CONFLICT | Backend port: `specs/api/hurl/run-hurl-tests.sh` defaults to `http://localhost:8000`; `specs/api/README.md` and `CLAUDE.md` examples use `http://localhost:3000/api`. | Port is a design decision (Phase 2). Not a product requirement. |
| ISS-003 | CONFLICT | `RealWorld apps Documentation.docx` is an older snapshot: it says "Over 150 implementations" and "Bootstrap 4 theme" and lists different maintainers, while the repository README says "Over 100" and refers to `assets/theme/styles.css`. | Use repository sources over the docx. |
| ISS-004 | AMBIGUOUS | Frontend routing docs say "Authentication can be easily switched to session/cookie based", while the E2E selector contract requires a LocalStorage JWT key. | Follow SRC-E2E (higher precedence); confirm in Phase 1. |
| ISS-005 | CONFLICT | RealWorld frontend and backend docs require at least one unit test per repository. The agreed stack names Playwright for frontend testing, which is mainly an E2E tool. | RESOLVED by DEC-003: Vitest for frontend unit tests. |
| ISS-006 | AMBIGUOUS | `expectations.md` contains ecosystem expectations (public GitHub repo with Issues open, framework with ≥300 stars, keep up to date). Their applicability to this learning project is undecided. | RESOLVED by DEC-007 (README = C-07, public repository, Issues enabled; star counts checked in Phase 2) and DEC-014 (maintain after the project, E-09). |
| ISS-007 | CONFLICT | The local NASA-STD-8739.8B PDF shows "Approved: 2022-09-08" and also contains the notice "This official draft has not been approved and is subject to modification. DO NOT USE PRIOR TO APPROVAL." | Use it as a learning reference only. Optionally replace it with the official copy from the NASA Technical Standards System. OPEN. |
| ISS-008 | AMBIGUOUS | Handbook version labels differ: the 2026-11-15 note and website URL say "Version D" (SWEHBVD), while the local Book A title material shows NASA-HDBK-2203B. The local export covers only six SWE guidance pages plus section listings (MTH-HB-LOCAL). | Treat MTH-HB-ONLINE as the full source; record the version label and page with each Handbook citation. OPEN. |
| ISS-009 | MISSING | NPR 7150.2D states an expiration date of 2027-03-08, which may fall within the project. | The planned QG5 (2027-07-06, `docs/00-governance/project-plan.md`) is after this date. Check NODIS for a successor before 2027-03-01 and record a decision to keep 7150.2D or move to the successor. OPEN. |
| ISS-010 | UNVERIFIABLE | The user approvals recorded in `docs/00-governance/decision-log.md` §1 are transcribed from the Kiro chat session, which is not stored in the repository. | CLOSED: the user confirmed the evidence register in the S1 decision (E-09, item 2). The chat transcript itself is still not stored in the repository. |
