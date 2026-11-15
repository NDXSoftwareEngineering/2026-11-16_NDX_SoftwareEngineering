# Source Inventory and Verification Report

Quality Gate S1, finding SG-03 and SG-04. Date: 2026-11-16. Document status: READY FOR REVIEW.
Performed by Kiro (self-check, not independent verification).

Status values used:
- VERIFIED: the check stated in the row was executed on 2026-11-16 and its output supports the result.
- MISSING: the item does not exist.
- NOT CHECKED: the item was not examined, or only the part stated was examined.

Raw evidence files:
- `docs/00-governance/evidence/realworld-integrity-2026-11-16.txt`
- `docs/00-governance/evidence/nasa-handbook-page-inventory-2026-11-16.txt`
- `docs/00-governance/evidence/cross-reference-check-2026-11-16.txt`

## 1. Path Verification (all paths referenced by the five steering files, `decision-log.md`, and `realworld-baseline.md`)

Method: a script extracted every back-quoted token that looks like a path from the seven files, resolved it against the repository root, `realworld-main/`, and `realworld-main/docs/src/content/docs/`, and resolved short file names by searching for a unique file of that name. Tokens that are not paths (`/api`, branch-name patterns, the example branch `feat/FR-ART-003-create-article`, a commit message, the `.docx` extension) were excluded.

### 1.1 Repository and governance files

| Path | Status | Note |
| --- | --- | --- |
| `.kiro/steering/project-overview.md` | VERIFIED | exists |
| `.kiro/steering/engineering-process.md` | VERIFIED | exists |
| `.kiro/steering/requirements-and-sources.md` | VERIFIED | exists |
| `.kiro/steering/technology-stack.md` | VERIFIED | exists |
| `.kiro/steering/quality-assurance.md` | VERIFIED | exists |
| `docs/00-governance/decision-log.md` | VERIFIED | exists; cross-references checked (§4) |
| `docs/00-governance/realworld-baseline.md` | VERIFIED | exists; claims re-checked (§2) |
| `docs/00-governance/qg-s1-review-package.md` | VERIFIED | created in this correction |
| `docs/01-requirements/` … `docs/05-deployment/` | MISSING (expected) | DEC-002: created when each phase starts |
| `2026-11-15 at 11.39.44 Note.txt` | VERIFIED | exists, tracked by Git |
| `_archive/realworld-main-download-2026-11-16/` | VERIFIED | exists, ignored by Git (`git check-ignore`) |
| `_archive/realworld-main-download-2026-11-16/RealWorld apps Documentation.docx` | VERIFIED | exists, 115,629 bytes, SHA-256 `530c82bb…483892` |
| `swebok-v4.pdf` | VERIFIED | exists, not tracked by Git |
| `package-lock.json`, `.env.example` | MISSING (expected) | future construction artifacts |

### 1.2 RealWorld paths (inside the `realworld-main/` submodule)

| Path | Status |
| --- | --- |
| `specs/`, `specs/api/openapi.yml`, `specs/api/README.md`, `specs/api/bruno/`, `specs/api/hurl/` (13 `.hurl` files), `specs/api/hurl/run-hurl-tests.sh`, `specs/api/run-api-tests-hurl.sh` | VERIFIED (exist) |
| `specs/api/hurl/errors_auth.hurl`, `specs/api/hurl/errors_authorization.hurl` | VERIFIED (exist) |
| `specs/e2e/` (12 `.spec.ts` files), `specs/e2e/SELECTORS.md`, `specs/e2e/playwright.base.ts`, `specs/e2e/xss-security.spec.ts` | VERIFIED (exist) |
| `assets/theme/styles.css` | VERIFIED (exists) |
| `CLAUDE.md` | VERIFIED (exists) |
| `docs/src/content/docs/**` (30 matches) | VERIFIED (exist) |
| `docs/src/content/docs/implementation-creation/features.md`, `…/expectations.md` | VERIFIED (exist) |
| `docs/src/content/docs/specifications/backend/introduction.md`, `endpoints.md`, `error-handling.md`, `cors.md` | VERIFIED (exist) |
| `docs/src/content/docs/specifications/frontend/routing.md` | VERIFIED (exists) |

### 1.3 Machine paths (`technology-stack.md` §2)

| Path | Status |
| --- | --- |
| `E:\2026-11-16 Cai PM o day\` and its `2026-11-16 Git\`, `2026-11-16 Nodejs\`, `2026-11-16 postgresql\bin\` folders | VERIFIED (exist) |
| `C:\Python314\` | VERIFIED (exists) |

Result: every referenced path that should exist today exists. No broken reference was found.

## 2. RealWorld Reference Integrity

| Check | Status | Result |
| --- | --- | --- |
| Superproject records submodule commit | VERIFIED | `25588846c314b15a5e427ffe816e6b4af7be68c7` |
| Submodule HEAD equals recorded commit | VERIFIED | equal |
| `.gitmodules` URL | VERIFIED | `https://github.com/realworld-apps/realworld` |
| No local modifications in submodule | VERIFIED | `git status --porcelain` empty |
| Object store integrity | VERIFIED | `git fsck --no-dangling` reported no errors |
| Inspected (archived) copy identical to pinned commit | VERIFIED | 337/337 Git blob hashes equal; 0 extra, 0 missing |
| Submodule working files identical to commit | VERIFIED with observation | 68 identical raw; 269 identical after CRLF→LF; 0 other differences (cause: `core.autocrlf=true` in Git for Windows system config) |
| OpenAPI version | VERIFIED | `openapi: 3.1.0`, `info.version: 2.0.0` |
| Claims in `realworld-baseline.md` (commit, date, message, counts 338/337/337/201) | VERIFIED | re-run matches |
| Pinned commit is still upstream HEAD | NOT CHECKED after initialization | upstream HEAD was `25588846` when the submodule was added on 2026-11-16 |

## 3. Content Inspection Record (what was actually read)

### 3.1 RealWorld sources (Group A)

| Source | Read in full | Read partly | Not read (NOT CHECKED) |
| --- | --- | --- | --- |
| Repository files | `README.md`, `CLAUDE.md`, `specs/api/README.md`, `specs/api/hurl/run-hurl-tests.sh`, `specs/e2e/playwright.base.ts` | `specs/api/openapi.yml` lines 1–40 only; `specs/e2e/SELECTORS.md` lines 1–60 plus a search for the debug interface and LocalStorage headings | All 13 `.hurl` files; all 12 `.spec.ts` files; `specs/e2e/helpers/`; `specs/api/bruno/`; `assets/theme/styles.css`; rest of `openapi.yml` and `SELECTORS.md` |
| Documentation site (`docs/src/content/docs/`) | `implementation-creation/features.md`, `implementation-creation/expectations.md`, `specifications/backend/introduction.md`, `specifications/backend/tests.md`, `specifications/backend/error-handling.md`, `specifications/frontend/tests.md`, `specifications/frontend/routing.md`, `specifications/frontend/api.md` | `specifications/backend/endpoints.md` (search hit for the `Authorization` header line only) | `cors.md`, `api-response-format.md`, `hurl.md`, `bruno.md`, `postman.md`, `frontend/templates.md`, `frontend/styles.md`, `mobile-specs/introduction.md`, `introduction.mdx`, `index.mdx`, community pages |
| `RealWorld apps Documentation.docx` | — | Text extracted; first 6,000 characters read; all headings listed | Remaining body text |

Steering statements that point to unread RealWorld files (for example "CORS behaviour follows `cors.md`") are pointers for later phases, not claims about their content.

### 3.2 NPR 7150.2D (local PDF, 89 pages, SHA-256 `1a5b7f9b…532802`)

| Part | Coverage |
| --- | --- |
| Whole document | Text extracted. Automated scan listed requirement sentences followed by a `[SWE-NNN]` tag. All 62 SWE IDs cited in the steering files were confirmed present as tags. |
| Read in context | Title page and table of contents; 2.2.1 (tailoring); 3.6.2 (SWE-141); 3.7.3 (SWE-134, opening); 3.9.2 (SWE-032, opening); 3.12.1 (SWE-052, Table 1 columns lost in extraction); 4.2.4 (SWE-143); 4.4.5–4.4.6 (SWE-062, SWE-186); 4.4.8 (SWE-136 and note); 4.5.2–4.5.3 (SWE-065, SWE-066); 4.5.6 (SWE-070); Appendix D Class D examples (partial), Class E and Class F definitions (partial) |
| Not read | Notes and lists of most other requirements; Chapters 1 and 2 beyond the statements; Appendices A–C and E; full Appendix D |

### 3.3 NASA-STD-8739.8B (local PDF, 70 pages, SHA-256 `e2fcf97e…d7a664`)

| Part | Coverage |
| --- | --- |
| Read | Pages 1–3 (title, approval date 2022-09-08, draft notice, revision history); paragraph defining technical, managerial, and financial independence of IV&V; table rows for SWE-141 and SWE-131 |
| Not read | All other content, including the software assurance task tables for other SWEs |

### 3.4 NASA Handbook: local partial export (MTH-HB-LOCAL) vs official online Handbook (MTH-HB-ONLINE)

| Item | Coverage |
| --- | --- |
| MTH-HB-LOCAL, 12 files, 151 pages | Page inventory: first 110 characters of every page (evidence file). This identifies structure only. |
| Book A (10 pages) | Pages 4 and 6 read partly: introduction text and title material ("NASA-HDBK-2203B, Approved: March 08, 2022"). Pages 2–3 show website posts dated October 2025. |
| Book B (18 pages), Book C (21 pages) | Page inventory only, plus Book C page 1 (first 500 characters). They list SWE titles and NPR requirement statements. Guidance content: NOT CHECKED. |
| Book D (4 pages), Book E (1 page) | Page inventory only. NOT CHECKED. |
| SWE-002 (two files), SWE-003, SWE-004, SWE-005, SWE-006, SWE-013 guidance pages | Page inventory only. Body text: NOT CHECKED. The two SWE-002 files have identical extracted page text and identical size (98,407 bytes) but different SHA-256 hashes. |
| MTH-HB-ONLINE (https://swehb.nasa.gov/spaces/SWEHBVD/overview) | NOT CHECKED. Not fetched. |

Conclusion: no Handbook guidance content was read. The steering files cite the Handbook only as a reference and for its version label, never for its guidance.

### 3.5 SWEBOK Guide v4.0a (local PDF, 411 pages, SHA-256 `0c9db4da…e511`, not committed)

| Part | Coverage |
| --- | --- |
| Read | Title page (v4.0a, released August 2026, editor Hironori Washizaki); chapter titles 1–18 |
| Not read | All chapter content. The phase-to-chapter mapping in `engineering-process.md` §11 is based on chapter titles only. |

### 3.6 Other sources

| Source | Status |
| --- | --- |
| `2026-11-15 at 11.39.44 Note.txt` | VERIFIED: read in full |
| GitHub repository settings | VERIFIED: `GET https://api.github.com/repos/NDXSoftwareEngineering/2026-11-16_NDX_SoftwareEngineering` returned `"private": false`, `"has_issues": true`, `"default_branch": "main"` (fetched twice on 2026-11-16) |
| Old OneDrive `.kiro` directory | NOT CHECKED by design: excluded by the user's instructions; not accessed |

## 4. Governance Document Cross-Check

From `docs/00-governance/evidence/cross-reference-check-2026-11-16.txt`:

| Check | Status | Result |
| --- | --- | --- |
| Every DEC ID referenced in the five steering files and `realworld-baseline.md` has a record in `decision-log.md` | VERIFIED | 0 undefined references |
| Every evidence ID (E-NN) used is defined in the evidence register | VERIFIED | 0 undefined |
| Decision statuses in the index match the records | VERIFIED by reading | DEC-001 to DEC-010 APPROVED; DEC-011 to DEC-013 PROPOSED; DEC-014 PENDING USER APPROVAL |
| Evidence quotes match the chat session | NOT CHECKED independently | Transcribed by Kiro; the transcript is not stored in the repository (ISS-010) |

## 5. Reference File Hashes (SHA-256)

Full values are in `docs/00-governance/evidence/cross-reference-check-2026-11-16.txt`. All 14 NASA PDFs and the 2026-11-15 note are tracked by Git; `swebok-v4.pdf` is not.
