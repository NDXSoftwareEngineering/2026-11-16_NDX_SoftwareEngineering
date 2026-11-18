# Phase 1 Status and Reading Record / Trạng thái và bản ghi đọc nguồn

Phase: 1 Requirements Engineering (QG1). Status: **IN PROGRESS**. Document status: DRAFT.
Started: 2026-11-18 (the planned start date in `docs/00-governance/project-plan.md`; confirmed by the system clock, 09:21 +07).
Vietnamese summary / Tóm tắt: Giai đoạn 1 đang chạy. Increment 1 (nguồn API) và increment 2 (nguồn frontend và E2E) đã xong. SRS, use case và RTM chưa được viết. Phát hiện lớn nhất: bộ E2E gắn với API công cộng của RealWorld (ISS-021).

## 1. Increment log

| Increment | Content | Status |
| --- | --- | --- |
| 1 | Source baseline, source reading, API source analysis, issues log | DONE (commit `bc093a9`) |
| 2 | Read E2E specs and helpers; frontend source analysis | DONE (this commit) |
| 3 | Draft SRS requirements for authentication, profiles, articles, comments, favorites, tags, errors | NOT STARTED |
| 4 | Draft frontend, non-functional, and constraint requirements; use cases | NOT STARTED |
| 5 | RTM v1, coverage checks, self-check, QG1 package | NOT STARTED |

## 2. Quality Gate QG1 entry criteria (from `engineering-process.md` §5)

| Criterion | Status |
| --- | --- |
| 19/19 OpenAPI operations map to a requirement | NOT RUN (no requirements yet) |
| 13/13 Hurl files map to a requirement | NOT RUN |
| 12/12 E2E spec files map to a requirement | NOT RUN |
| 100% requirements have a source or CON/PROP label | NOT RUN |
| 100% requirements have a verification method | NOT RUN |
| No unmeasured vague wording | NOT RUN |
| Every open issue has a disposition or user acceptance | NOT RUN (14 issues, ISS-011 to ISS-024: 13 open, 1 answered; decided together at the QG1 review, DEC-017) |

## 3. Reading record (what was actually read on 2026-11-18)

"Read in full" means the file content was displayed and read. "Titles only" means only test names were extracted by script. "Not read" means not opened.

### 3.1 Read in full

| Source | Files |
| --- | --- |
| SRC-API | `specs/api/openapi.yml` (926 lines) |
| SRC-HURL | all 13 files: `articles`, `auth`, `comments`, `errors_articles`, `errors_auth`, `errors_authorization`, `errors_comments`, `errors_profiles`, `favorites`, `feed`, `pagination`, `profiles`, `tags` |
| SRC-E2E | `specs/e2e/SELECTORS.md`, `specs/e2e/playwright.base.ts`, all 8 files in `specs/e2e/helpers/`, and these spec files: `articles`, `auth`, `comments`, `health`, `navigation`, `null-fields`, `settings`, `social`, `url-navigation`, `user-fetch-errors`, `xss-security` |
| SRC-DOC (backend) | `endpoints.md`, `api-response-format.md`, `error-handling.md`, `cors.md`, `hurl.md`, `introduction.md`, `tests.md` |
| SRC-DOC (frontend) | `templates.md`, `routing.md`, `styles.md`, `api.md`, `tests.md` |
| SRC-DOC (general) | `implementation-creation/features.md`, `expectations.md`, `introduction.md` |
| Repository | `README.md`, `CLAUDE.md`, `specs/api/README.md`, `specs/api/hurl/run-hurl-tests.sh` |

### 3.2 Partly read

| Source | Files |
| --- | --- |
| SRC-E2E | `specs/e2e/error-handling.spec.ts` (911 lines): lines 1 to 135 read in full; the other lines only by test titles and by a search of all `page.route` calls. The bodies of 27 tests are not read. |

### 3.3 Not read

| Source | Files | Reason |
| --- | --- | --- |
| SRC-THEME | `assets/theme/styles.css` | Visual theme; to be used as a reference in design, not read for requirements |
| SRC-BRUNO | `specs/api/bruno/` | Generated from Hurl; not an independent source |
| SRC-DOC | `bruno.md`, `postman.md`, `mobile-specs/introduction.md`, `introduction.mdx`, `index.mdx`, `community/*` | Tooling pages, mobile (out of scope), and community pages; no product requirement expected. Not confirmed by reading. |
| SRC-DOCX | `RealWorld apps Documentation.docx` | Lowest precedence; headings listed on 2026-11-16, body not read |

## 4. Source baseline

| Item | Value |
| --- | --- |
| Upstream repository | https://github.com/realworld-apps/realworld |
| Pinned commit | `25588846c314b15a5e427ffe816e6b4af7be68c7` (submodule `realworld-main/`) |
| Manifest | `evidence/source-manifest-2026-11-18.txt`: Git blob ids of 228 files (`specs/`, `docs/src/content/docs/`, theme, README, CLAUDE, CONTRIBUTING) |
| Integrity | Verified for Quality Gate S1 (`docs/00-governance/realworld-baseline.md`); not re-run today. The submodule status can be re-checked with `git submodule status`. |

## 5. Measured source size (corrects the plan)

| Source | Measure | Result (script, 2026-11-18) |
| --- | --- | --- |
| OpenAPI | operations | 19 (12 paths) |
| Hurl | files / requests | 13 / 157 |
| E2E | files / static `test()` declarations / `describe` groups | 12 / 133 / 22 |

Additional measure (increment 2): the XSS tests expand from 5 static declarations to an estimated 16 runtime tests, so the estimated runtime total is 144 (`frontend-source-analysis.md` §2).

Correction C-01: `project-plan.md` v1.0 stated "about 170 tests" for E2E. That figure was the sum of 133 tests, 22 `describe` groups, and 15 hooks, which my first script counted together. The correct static count is 133. The runtime test count is higher because the XSS tests iterate over 11 payloads (see `xss-security.spec.ts`); the exact runtime count comes from the Playwright run in Phase 4. The plan is corrected in v1.1 and the user is asked to confirm.

## 6. Increment 2 result

| Item | Result |
| --- | --- |
| New document | `frontend-source-analysis.md` |
| ISS-020 | Answered: the "400" tests use mocked responses |
| New issues | ISS-021 (E2E bound to the public demo API and to a run mode), ISS-022 (seed data), ISS-023 (backend stores XSS payloads unchanged), ISS-024 (`SELECTORS.md` incomplete) |
| Effect on the steering baseline | `quality-assurance.md` QO-1 and `engineering-process.md` §8 state "100% of RealWorld Playwright tests pass" without a mode. The issue is carried to the QG1 review (ISS-021); no steering text was changed. |

## 7. Next step

Increment 3: draft the SRS for the API side (authentication, profiles, articles, comments, favorites, tags, errors) from `api-source-analysis.md`, with each requirement traced to a source and carrying the recommended option of every related issue as a proposal (DEC-017).
