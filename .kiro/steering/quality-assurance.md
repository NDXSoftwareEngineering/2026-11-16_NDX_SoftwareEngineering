---
inclusion: always
---

# Quality Assurance

Baseline version: 1.3 (2026-11-16). Status: APPROVED by the user. v1.0 approved 2026-11-16 (DEC-001); decisions DEC-002 to DEC-008 applied in v1.2; v1.1 NASA and SWEBOK references and NASA applicability approved in v1.3 (DEC-009). Decisions: `docs/00-governance/decision-log.md`.

Methodology basis: NASA-STD-8739.8B (software assurance tasks and IV&V concepts), NPR 7150.2D (SWE-022 plan and implement software assurance per NASA-STD-8739.8; verification and validation requirements listed in `engineering-process.md` §11), NASA-HDBK-2203 (guidance), and SWEBOK v4.0a Ch 5 Software Testing, Ch 8 Software Configuration Management, Ch 12 Software Quality, and Ch 13 Software Security. SWE numbers are cited only after they have been checked in the source.

| QA topic | NPR 7150.2D references |
| --- | --- |
| Reviews | SWE-087, SWE-088, SWE-089 (peer reviews and inspections, with measurements) |
| Static analysis | SWE-135 (static analysis for defects, security, code coverage, complexity); SWE-185 (secure coding verified by static analysis) |
| Unit testing | SWE-062, SWE-186 (repeatable results) |
| Test documentation and execution | SWE-065, SWE-066, SWE-068, SWE-071 |
| Coverage and regression | SWE-189, SWE-190, SWE-191 |
| Security | SWE-154, SWE-156, SWE-159, SWE-207 |
| Defects (non-conformances) | SWE-201, SWE-202, SWE-204 |
| Configuration management | SWE-079 to SWE-085, SWE-187 |
| Traceability | SWE-052 |

## 1. Proportionate Application of NASA Assurance

This is a single-developer learning project with no safety-critical function. NASA practices are applied in a scaled form:

- Kept: planned assurance activities per phase, documented reviews, requirements-based verification, traceability, defect tracking, configuration control, objective evidence, and explicit acceptance.
- Scaled down: formal assurance plans become sections of these steering files and phase documents; review boards become a user review.
- Not claimed: software safety analysis beyond basic security and data-integrity checks, and IV&V.

IV&V applicability: NPR 7150.2D SWE-141 requires IV&V only for certain NASA projects: Category 1 projects, Category 2 projects with Class A or B payload risk (categories per NPR 7120.5), and projects explicitly selected by a NASA Mission Directorate. This project has no such category, so IV&V is not required and is not performed. NASA-STD-8739.8B defines three parts of IV&V independence: technical, managerial, and financial. Technical independence means the people doing the IV&V analysis are not involved in developing the system.

Independence rule: Kiro reviewing its own artifacts is a self-check by the developer. It is not independent assurance and not IV&V. Every review record states who reviewed and whether the review was independent. The user's review is the acceptance review. Do not describe any activity as IV&V unless a party independent of the development performed it.

## 2. Quality Objectives

| ID | Objective | Measure |
| --- | --- | --- |
| QO-1 | Functional conformance | 100% of RealWorld Hurl tests and 100% of RealWorld Playwright E2E tests pass, with recorded output |
| QO-2 | Requirements coverage | 100% of baselined requirements traced to verification with a status and evidence link |
| QO-3 | Code quality | Zero unresolved static analysis errors at QG3 and QG4 |
| QO-4 | Security | No open High/Critical security findings; no secrets in the repository; XSS E2E tests pass |
| QO-5 | Reproducibility | Build, tests, and deployment reproducible from documented steps on a clean checkout |
| QO-6 | Defect control | Zero open Critical/High defects at QG4 and QG5 |

Code coverage thresholds are not set by RealWorld. Any coverage target is a proposed decision for Phase 2, not a requirement.

## 3. Quality Planning

- Each phase document states the planned verification activities, their entry and exit criteria, and the evidence to be produced, before the work starts.
- The test strategy is outlined in Phase 2 and detailed as a test plan at the start of Phase 4. Unit tests are written during Phase 3.
- Quality gate criteria are those in `engineering-process.md` plus §15 below.

## 4. Verification and Validation

- Verification ("built right"): reviews of each artifact against its source and checklist; tests against requirements; traceability checks.
- Validation ("right product"): demonstration of the running application through the RealWorld user flows (register, log in, write and edit articles, comment, favorite, follow, settings, logout), reviewed by the user.
- Each requirement has one verification method: Test, Analysis, Inspection, or Demonstration, set in Phase 1.

## 5. Reviews

| Artifact | Review type | Minimum checklist |
| --- | --- | --- |
| SRS, RTM | Self-check, then user review | Source coverage, quality attributes of each requirement, open issues dispositioned |
| SDD, decision records | Self-check, then user review | Every requirement covered, no orphans, decisions justified, security covered |
| Code increments | Self-check, user review of selected increments | Matches design, tests present, static analysis clean, no secrets |
| Test reports | Self-check, then user review | Raw output attached, counts consistent, failures linked to defects |
| Deployment guide | User review and dry-run where practical | Steps reproducible, configuration documented, smoke tests defined |

Review findings are recorded with ID, description, severity, and resolution.

## 6. Static Analysis

- Run from Phase 3 onward, on every increment, before each gate.
- Tools (DEC-005): Ruff for Python; ESLint and Prettier for JavaScript/TypeScript; pip-audit and `npm audit` for dependencies; TypeScript compiler checks if TypeScript is chosen; Django `manage.py check` (built into Django, including `--deploy` before Phase 5).
- Findings are fixed or justified in writing. Suppressions must name the rule and the reason.

## 7. Unit Testing

- Backend: pytest. Unit tests cover domain logic, validation, and permission rules (business rules BR-*).
- Frontend: Vitest (DEC-003). RealWorld expects at least one unit test per repository; this project aims higher, with the target set in Phase 2.
- Tests are deterministic, isolated, and runnable with one documented command.
- Test names or docstrings reference requirement IDs.

## 8. Integration Testing

- Backend: pytest tests that exercise Django Ninja endpoints with a real PostgreSQL test database (not SQLite), covering request validation, persistence, authentication, and authorization.
- Frontend–backend: covered by the E2E suite running against the real backend and database.

## 9. API Testing

- Conformance: run the full RealWorld Hurl suite (`realworld-main/specs/api/hurl/`) against the local backend, with sequential execution (`--jobs 1`) and a unique `uid` variable per run, as the RealWorld runner script does.
- The Hurl files are not modified. A failing Hurl test is a defect in the implementation, unless the user approves an issue stating the test itself is wrong.
- Evidence: full Hurl console output and the command used, saved with the test report.

## 10. End-to-End Testing

- Run the RealWorld Playwright suite (`realworld-main/specs/e2e/`) by extending `playwright.base.ts` with this project's `baseURL` and `webServer` settings, without editing the spec files.
- The frontend must satisfy `SELECTORS.md`.
- Evidence: Playwright HTML report and console summary, with the browser and versions used.

## 11. Security Testing

Scaled to the project; based on RealWorld security-relevant behaviour and common web risks:

- Authentication and authorization: Hurl `errors_auth.hurl` and `errors_authorization.hurl`, plus pytest tests for 401/403 rules.
- XSS: RealWorld `xss-security.spec.ts`; review of the Markdown rendering and sanitization design.
- Input validation: 422 error format and messages per `error-handling.md`.
- Secrets: repository scan for committed secrets before each gate from QG3.
- Dependencies: vulnerability audit of Python and npm dependencies before QG3, QG4, and QG5.
- Configuration: Django deployment checks before QG5 (debug off, secure secret key, allowed hosts, CORS limited to known origins).

## 12. Defect Management

- Each defect `DEF-NNN` records: title, steps to reproduce, expected vs actual result, environment, severity, related requirement and test, status, fix commit, and re-test evidence.
- Severity: Critical (blocks core use or data loss/security breach), High (feature not working, no workaround), Medium (feature works with workaround or wrong behaviour in a non-core path), Low (cosmetic).
- Status flow: OPEN → IN PROGRESS → FIXED → VERIFIED → CLOSED, or REJECTED / DEFERRED with user approval.
- A defect is CLOSED only after re-test evidence shows the fix and regression tests pass.
- Location [RECOMMENDATION]: GitHub Issues once the repository exists; until then a defect log in `docs/04-testing/`.

## 13. Configuration Management

- Configuration items: steering files, phase documents, RTM, decision records, source code, migrations, test code, dependency lock files, environment example files, and the RealWorld reference baseline.
- Baselines: the requirements baseline at QG1, design baseline at QG2, code baseline at QG3 (Git tag), tested release at QG4 (Git tag), deployed release at QG5 (Git tag).
- Changes to baselined items follow change control in `requirements-and-sources.md`.
- The RealWorld reference copy is identified by file hashes recorded in Phase 1 (ISS-001), because no upstream commit is known.

## 14. Quality Evidence

Evidence must be objective and reviewable:

- Raw tool output (console logs, reports) with the exact command, date, Git commit, and tool versions.
- Evidence files stored with the phase documentation, or linked from it.
- Summaries must match the raw output. Counts (passed/failed/skipped) are copied from tool output, not estimated.
- If a check could not be run, report it as NOT RUN or BLOCKED with the reason. Never report a result that was not produced.

## 15. Quality Gates and Acceptance

Each gate QG1–QG5 passes only when all of the following hold:

1. All exit criteria in `engineering-process.md` for the phase have status PASS with evidence, or an N/A / carried-forward disposition the user accepted explicitly.
2. The RTM coverage checks for the phase pass (forward, backward, and source coverage).
3. No open Critical/High defects or review findings for the phase's artifacts.
4. All decisions made during the phase are recorded and approved.
5. The gate record lists open issues, reviewers, and review independence.
6. The user records ACCEPTED in the gate record. The agent never records acceptance.
