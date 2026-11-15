---
inclusion: always
---

# Engineering Process: Mandatory Five-Phase Lifecycle

Baseline version: 1.3 (2026-11-16). Status: APPROVED by the user. v1.0 approved 2026-11-16 (DEC-001); decisions DEC-002 to DEC-008 applied in v1.2; v1.1 NASA and SWEBOK references and NASA applicability approved in v1.3 (DEC-009). Decisions: `docs/00-governance/decision-log.md`.

Methodology references: NPR 7150.2D (software engineering requirements and lifecycle activities), NASA-STD-8739.8B (software assurance, safety, IV&V), NASA Software Engineering Handbook (implementation guidance), SWEBOK Guide V4 (knowledge areas). These guide how work is done. They never add product requirements.

## 1. Operating Rules (apply in every phase)

1. Work sequentially. Start a phase only after the previous gate is accepted.
2. Never skip a quality gate. Never mark one passed on the user's behalf.
3. Never implement functionality before its requirements are approved (QG1) and its design is approved (QG2).
4. Maintain bidirectional traceability: requirement ↔ design element ↔ code ↔ test ↔ result.
5. Record every important engineering decision as a decision record (see §4).
6. Give every deliverable measurable acceptance criteria.
7. Never fabricate test results, tool output, or review findings.
8. Never mark an activity PASS without evidence a reviewer can open.
9. Prefer simple, maintainable architecture. RealWorld's own expectation is an "early-stage startup MVP: functionally complete and stable, but not over-engineered" (`implementation-creation/expectations.md`).
10. Do not introduce frameworks or infrastructure that no approved requirement or decision needs.
11. Ask for approval before any change to scope, architecture, requirements, or technology.
12. Label statements as FACT (verified, with source), ASSUMPTION, RECOMMENDATION, DECISION (approved, with ID), or ISSUE (unresolved, with ID).

## 2. Status Vocabulary

| Status | Meaning |
| --- | --- |
| PASS | Executed; evidence shows the criterion is met. |
| FAIL | Executed; evidence shows the criterion is not met. |
| NOT RUN | Not executed yet. |
| BLOCKED | Cannot be executed; blocking issue ID given. |
| N/A | Not applicable; reason given and accepted by the user. |

Phase status values: NOT STARTED → IN PROGRESS → READY FOR REVIEW → ACCEPTED (by user) or REWORK.

## 3. Documentation Location

[DECISION DEC-002] Lifecycle documents live under `docs/` at the project Git repository root, one folder per phase: `docs/00-governance/`, `docs/01-requirements/`, `docs/02-design/`, `docs/03-construction/`, `docs/04-testing/`, `docs/05-deployment/`. Steering files stay in `.kiro/steering/`. Create each phase folder when that phase starts. Decision records are kept in `docs/00-governance/decision-log.md`.

## 4. Decision Records and Gate Records

Decision record (`DEC-NNN`), one per important decision: context, options considered, decision, rationale, consequences, status (PROPOSED / APPROVED / SUPERSEDED), approver, date.

Gate record (`QGn`), one per phase gate:
- Checklist of exit criteria, each with status and evidence link.
- Open issues carried forward, each with the user's explicit acceptance.
- Reviewer(s) and review type (self-check by agent, or user review).
- User decision: ACCEPTED or REWORK, with date. Only the user writes this field.

## 5. Phase 1: Requirements Engineering (QG1)

Objectives: Produce a complete, correct, verifiable, source-traced requirements baseline for Conduit.

Activities:
- Inventory and baseline the RealWorld sources (record what was inspected and fingerprints).
- Elicit requirements by analysing the OpenAPI spec, Hurl suite, E2E suite and selector contract, and docs prose.
- Classify requirements (functional, non-functional, business rule, interface, constraint) per `requirements-and-sources.md`.
- Write use cases for the user-facing features.
- Record ambiguities, conflicts, gaps, and unverifiable statements as issues.
- Build RTM v1 (requirement → source).

Inputs: RealWorld sources; these steering files; user decisions.

Outputs: SRS; use cases; RTM v1; issues and assumptions log; source baseline record.

Required documentation: `docs/01-requirements/` SRS, use cases, RTM, issues log.

Review activities: Agent self-check against the checklist below; user walkthrough of the SRS and all open issues.

Acceptance criteria:
- 100% of OpenAPI operations map to at least one requirement.
- 100% of Hurl test files and E2E spec files map to at least one requirement.
- 100% of requirements have a source reference, or are labelled engineering constraint (CON) or proposed decision (PROP).
- Every requirement has a verification method (test, analysis, inspection, or demonstration).
- No requirement uses unverifiable wording (for example "fast", "user-friendly") without a measurable criterion.
- Every open issue has an owner and a resolution or an explicit user acceptance to carry it forward.

Quality Gate QG1: User accepts the SRS, RTM v1, and issue dispositions. Requirements are then baselined; later changes follow change control.

## 6. Phase 2: Software Design (QG2)

Objectives: Define an architecture and detailed design that satisfy the baselined requirements on the agreed stack.

Activities:
- Architecture: components, responsibilities, interfaces, deployment view.
- Data design: entities, relationships, constraints, migrations approach.
- API design: mapping from OpenAPI operations to backend modules; authentication; error format.
- UI design: routes, pages, components, state handling, selector contract compliance.
- Security design: authentication, authorization, input validation, XSS handling, secrets.
- Test strategy outline and verification approach per requirement.
- Decide versions, tool choices, ports, deployment target (decision records).
- Extend RTM to v2 (requirement → design element).

Inputs: Baselined SRS and RTM v1; `technology-stack.md`.

Outputs: Software Design Description (SDD); decision records; RTM v2; test strategy outline.

Required documentation: `docs/02-design/` SDD, DEC records, RTM v2.

Review activities: Agent self-check; user design review covering every requirement and every decision record.

Acceptance criteria:
- 100% of baselined requirements map to at least one design element.
- No design element exists without a requirement or approved decision (no gold plating).
- Every technology or version choice is recorded in an approved decision record.
- Security design covers each security-related requirement.

Quality Gate QG2: User accepts the SDD, decision records, and RTM v2.

## 7. Phase 3: Software Construction (QG3)

Objectives: Implement the approved design with maintainable, version-controlled, unit-tested code.

Activities:
- Set up repositories, environments, and dependency pinning per approved decisions.
- Implement in small increments, each referencing requirement IDs in commits.
- Write unit tests alongside code.
- Run static analysis and fix findings or record justified exceptions.
- Extend RTM to v3 (design element → code module → unit test).

Inputs: Approved SDD and RTM v2; decision records.

Outputs: Source code in Git; unit tests; build/run instructions (README); static analysis reports; RTM v3.

Required documentation: `docs/03-construction/` build notes, static analysis summary, RTM v3; README in the repository.

Review activities: Agent self-check of each increment; user code review of selected increments; review of static analysis output.

Acceptance criteria:
- Application builds and starts from a clean checkout by following the README, demonstrated with recorded commands and output.
- All unit tests pass (recorded run output).
- Static analysis has zero unresolved errors; warnings are fixed or justified.
- No secrets in the repository.
- 100% of baselined requirements have implementing code or a recorded deferral approved by the user.

Quality Gate QG3: User accepts the construction evidence and RTM v3.

## 8. Phase 4: Software Testing (QG4)

Objectives: Verify the product against requirements and validate it against intended use, with recorded evidence.

Activities:
- Write the test plan (scope, levels, environment, entry/exit criteria).
- Execute unit, integration, API (Hurl), and E2E (Playwright) tests; execute security checks.
- Log defects, fix, and re-test (regression).
- Extend RTM to v4 with verification status per requirement.
- Write the test summary report.

Inputs: QG3-accepted build; SRS; RTM v3; test strategy.

Outputs: Test plan; test cases; execution reports with raw output; defect log; RTM v4; test summary report.

Required documentation: `docs/04-testing/`.

Review activities: Agent self-check of evidence completeness; user review of the test summary report and all open defects.

Acceptance criteria:
- RealWorld Hurl API suite: all tests pass against the backend (recorded output).
- RealWorld Playwright E2E suite: all tests pass against the frontend (recorded report).
- 100% of baselined requirements have verification status PASS, or a documented FAIL/BLOCKED disposition accepted by the user.
- No open defect of severity Critical or High.

Quality Gate QG4: User accepts the test summary report, defect dispositions, and RTM v4.

## 9. Phase 5: Deployment (QG5)

Objectives: Package, deploy, configure, and operationally check the application in the approved target environment.

Activities:
- Prepare the release (tag, release notes, version).
- Prepare environment configuration without committing secrets.
- Deploy following a written deployment guide.
- Run smoke tests and operational checks in the target environment.
- Write the final project report and lessons learned.

Inputs: QG4-accepted release candidate; deployment target decision.

Outputs: Deployment guide; configuration description; release tag; smoke test report; final report.

Required documentation: `docs/05-deployment/`.

Review activities: User review of the deployment guide and smoke test evidence; user dry-run of the guide where practical.

Acceptance criteria:
- Deployment is reproducible by following the guide, shown by recorded execution.
- Smoke tests pass in the target environment (recorded output).
- Release is tagged in Git and matches the tested build.
- Final report lists unresolved issues and lessons learned.

Quality Gate QG5: User accepts the deployment and final report. The project is closed.

## 10. Rework and Re-entry

If a later phase finds a defect in an earlier artifact (for example a requirements error found in testing), raise a change request (see `requirements-and-sources.md`), update the earlier artifact, and re-run the affected review. Earlier gates are not re-opened silently.

## 11. Methodology Mapping (verified references)

NPR 7150.2D SWE numbers below were checked against the local PDF text. They are learning references for each phase, applied proportionately (see `quality-assurance.md` §1). They are not product requirements and do not imply NASA compliance. SWEBOK references are chapters of SWEBOK Guide v4.0a.

| Phase / topic | NPR 7150.2D references | SWEBOK v4.0a |
| --- | --- | --- |
| Planning and governance (pre-phase) | SWE-013 software plans; SWE-024 track actual results against plans; SWE-034 define acceptance criteria; SWE-037 define review milestones; SWE-121 document approved tailoring; SWE-125 requirements mapping matrix; SWE-020 classification (Appendix D) | Ch 9 Engineering Management; Ch 10 Engineering Process |
| 1. Requirements | SWE-050 establish and approve requirements; SWE-051 requirements analysis; SWE-053 manage requirement changes; SWE-054 track inconsistencies to closure; SWE-055 requirements validation; SWE-052 bidirectional traceability; SWE-200 requirements volatility | Ch 1 Software Requirements |
| 2. Design | SWE-057 recorded software architecture; SWE-143 architecture review (required only for Category 1/2 projects; done here as a project choice); SWE-058 detailed design; SWE-027 conditions for using OSS/reused components; SWE-154, SWE-156 cybersecurity risks and assessment | Ch 2 Software Architecture; Ch 3 Software Design; Ch 13 Software Security |
| 3. Construction | SWE-060 implement design in code; SWE-061 coding standards; SWE-207 secure coding practices; SWE-135 static analysis; SWE-185 verify secure coding with static analysis; SWE-062 unit test; SWE-186 repeatable unit test results | Ch 4 Software Construction |
| 4. Testing | SWE-065 test plans, procedures, tests, reports; SWE-066 test against requirements; SWE-187 configuration management before testing; SWE-068 evaluate and record test results; SWE-071 keep test plans consistent with requirements; SWE-189, SWE-190 code coverage; SWE-191 regression testing; SWE-159 test cybersecurity mitigations | Ch 5 Software Testing; Ch 12 Software Quality |
| 5. Deployment | SWE-073 validate on the target platform; SWE-063 version description per release; SWE-085 release and delivery procedures; SWE-075 operations and maintenance planning; SWE-077 deliver product with as-built records | Ch 6 Software Engineering Operations; Ch 7 Software Maintenance |
| Cross-cutting | SWE-079 to SWE-084 configuration management; SWE-087 to SWE-089 peer reviews and inspections; SWE-090, SWE-093 measurement; SWE-201, SWE-202 non-conformance tracking and severity levels; SWE-204 process assessment for high-severity non-conformances; SWE-022 assurance per NASA-STD-8739.8 | Ch 8 Configuration Management; Ch 12 Software Quality; Ch 14 Professional Practice |

Proportionate interpretation for this project:
- Project plans (SWE-013) are these steering files plus phase documents. A separate plan document is not required.
- The requirements mapping matrix (SWE-125) and the classification exercise (SWE-020) were deferred by the user on 2026-11-16. If revisited, the mapping matrix is produced and lists each referenced SWE as Applied, Tailored, or Not applicable, with a reason.
- Classification (SWE-020) is optional. The closest Appendix D definitions are Class E (design concept, research, and general purpose software) and Class F (general purpose computing, business, and IT software). Conduit is not NASA software, so any class assigned is a learning exercise and needs user approval.
- [DECISION DEC-009] Not applicable: SWE-032 (CMMI-DEV rating), SWE-070 (accredited models and simulations for flight software qualification), SWE-141 and related IV&V requirements (SWE-131, SWE-178, SWE-179), and safety-critical requirements (SWE-023, SWE-134, SWE-184, SWE-192). Reason: no NASA project category, no organization with a CMMI rating, no flight software, and no safety-critical or mission-critical software.
- [DECISION DEC-009] SWE-136 (validate and accredit development tools) is applied in reduced form, without formal accreditation: record the version of every tool in test and build evidence; use only stable, maintained releases; pin versions in the Python requirements file and `package-lock.json`.
- [DECISION DEC-009] SWE-143 (architecture review) is required by NPR 7150.2D only for Category 1 projects and Category 2 projects with Class A or B payload risk. This project still reviews its architecture at QG2, as a project choice, not because SWE-143 requires it.

Limits of this mapping (accepted by the user, DEC-009):
- Assigning SWE numbers and SWEBOK chapters to phases is the agent's engineering judgment, not a mapping published by NASA or SWEBOK.
- NPR 7150.2D SWE-052 Table 1 (traceability by software class) lost its column layout during PDF text extraction. This project does not cite it per class; it applies full bidirectional traceability as a project choice.
