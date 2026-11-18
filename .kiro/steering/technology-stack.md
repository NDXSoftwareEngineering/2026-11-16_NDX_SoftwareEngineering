---
inclusion: always
---

# Technology Stack

Baseline version: 1.4 (2026-11-16). Status: APPROVED by the user on 2026-11-16 with the acceptance of Quality Gate S1 (evidence E-09). After that approval only status markers were updated to record E-09. History and evidence: `docs/00-governance/decision-log.md` §1 and §4. Exception: §11 (language rule) was reworded on 2026-11-16 to apply DEC-016; that wording is READY FOR REVIEW.

## 1. Change Policy

The agreed stack is: Windows 11, Kiro, Python 3.12, Django, Django Ninja, React, Vite, PostgreSQL 17, pytest, Playwright, Git, GitHub.

Stack status (Quality Gate S1, SG-02). Evidence IDs refer to `docs/00-governance/decision-log.md` §1.

| Item | Role | Status | Evidence |
| --- | --- | --- | --- |
| Windows 11, Kiro, Python 3.12, Django, Django Ninja, React, Vite, PostgreSQL 17, pytest, Playwright, Git, GitHub | Original agreed stack | AGREED (defined by the user in the original task) | User task statement |
| Vitest | Frontend unit tests | APPROVED (DEC-003) | E-02 |
| Hurl for Windows, run from PowerShell | RealWorld API conformance suite | APPROVED (DEC-004) | E-02 |
| Ruff, ESLint, Prettier, pip-audit | Static analysis, formatting, Python dependency audit | APPROVED (DEC-005) | E-02 |
| Python `venv` + `pip`, pinned requirements file | Python dependency management | APPROVED (DEC-005) | E-02 |
| Git submodule for the RealWorld reference | Reference baseline | APPROVED (DEC-006 b; path and commit: DEC-012) | E-02, E-09 |
| Node.js runtime, npm, `package-lock.json`, `npm audit` | Frontend toolchain prerequisite | APPROVED (DEC-013) | E-09 |
| pytest Django integration plugin | Backend test integration | Not yet proposed; Phase 2 decision | None |
| JavaScript or TypeScript, router, Markdown renderer, JWT library, HTTP client | Application libraries | Not yet proposed; Phase 2 decisions | None |
| Python 3.14 with pypdf 5.1.0 | Reading reference PDFs only; not part of the product stack | Installed by the user; usage recorded for SWE-136 (reduced) | User statement E-01 ("Tôi đã cài pypdf cho bạn") |

- Do not add, replace, or remove a framework, library category, tool, or infrastructure component without explicit user approval recorded as a decision record (`DEC-NNN`).
- Small libraries needed to implement an approved design (for example a JWT library or a Markdown renderer) are proposed in the Design phase with a decision record. They are not added ad hoc during construction.
- Docker and WSL2 are deferred. Do not introduce them, or bash-only workflows, without approval.
- Items marked [RECOMMENDATION] below are not approved. Treat them as proposals until a DEC approves them.

## 2. Development Environment

Observed on 2026-11-16 (FACT, from command output in this workspace):

| Item | Observation |
| --- | --- |
| OS | Microsoft Windows 11 Home Single Language |
| Shell | PowerShell |
| Python | 3.12.10, pip 25.0.1, installed under `E:\2026-11-16 Cai PM o day\` |
| Node.js / npm | Node v24.22.3 with npm, under `E:\2026-11-16 Cai PM o day\2026-11-16 Nodejs\` |
| Git | Installed, under `E:\2026-11-16 Cai PM o day\2026-11-16 Git\` |
| PostgreSQL | Windows service `postgresql-x64-17` is running; binaries in `E:\2026-11-16 Cai PM o day\2026-11-16 postgresql\bin\`; `psql` is not on PATH |
| Second Python | Python 3.14 at `C:\Python314\` is the default for the `py` launcher. It has `pypdf` 5.1.0, used only to read reference PDFs. It is not part of the project stack. Project commands must use Python 3.12 explicitly (`python` on PATH, or `py -3.12`). |
| Not found on PATH | `hurl`, `bash`, `bun`, `gh`, `uv` |
| Workspace | `E:\2026-11-16-SoftwareEngineering` is the Git repository root, branch `main` (DEC-008). Remote `origin`: https://github.com/NDXSoftwareEngineering/2026-11-16_NDX_SoftwareEngineering (public), first pushed 2026-11-16. |

Rules:
- Tool paths contain spaces. Always quote paths in commands.
- Use PowerShell syntax for scripts and documented commands. Do not assume bash.
- Long-running processes (dev servers, watchers) are started by the user in their own terminal, or as managed background processes, never as blocking commands.

## 3. Backend

- Python 3.12 with Django and Django Ninja providing the JSON REST API defined by `realworld-main/specs/api/openapi.yml`.
- Django ORM and Django migrations for data access and schema evolution.
- Architecture style: one Django project with a small number of Django apps grouped by RealWorld domain area. The exact module breakdown is a Phase 2 design output.
- Authentication: JWT sent as `Authorization: Token <jwt>` (FACT, `docs/.../specifications/backend/endpoints.md`). The JWT library and token settings are Phase 2 decisions.
- CORS behaviour follows `docs/.../specifications/backend/cors.md`, designed in Phase 2.
- Error responses follow `docs/.../specifications/backend/error-handling.md` and the Hurl error suites (422 with `{"errors": {...}}`, plus 401, 403, 404).
- Versions: Django and Django Ninja versions are pinned by a Phase 2 decision record. Prefer releases that support Python 3.12 and remain supported for the project duration.

## 4. Frontend

- React single-page application built with Vite.
- Must satisfy RealWorld routing, templates, shared theme (`realworld-main/assets/theme/styles.css`), and the E2E selector contract (`realworld-main/specs/e2e/SELECTORS.md`), including the JWT LocalStorage key and `window.__conduit_debug__` interface defined there.
- Language (JavaScript or TypeScript), router, and Markdown rendering library are Phase 2 decisions.
- Article Markdown is rendered client-side (FACT, `routing.md`); XSS protection is a mandatory design concern (the E2E suite includes `xss-security.spec.ts`).

## 5. Database

- PostgreSQL 17, accessed only through the Django ORM.
- Separate databases for development and automated tests. Test databases are created and destroyed by the test tooling.
- Schema changes are made only through committed Django migrations.
- The data model is a Phase 2 output. Do not create schema before QG2.

## 6. API Integration

- The OpenAPI file and Hurl suite are the integration contract between frontend and backend.
- The frontend calls the backend over HTTP using a configurable base URL ending in `/api`.
- Backend and frontend ports are a Phase 2 decision (see ISS-002 in `requirements-and-sources.md`).
- The public demo API (`https://api.realworld.show/api`) may be used for exploratory checks only. It is not a verification target for this project's backend.

## 7. Version Control

- Git, hosted on GitHub. One repository (monorepo) holds backend, frontend, and documentation (DEC-006 a). GitHub repository: `2026-11-16_NDX_SoftwareEngineering`, public (DEC-006 c).
- Branching [RECOMMENDATION]: `main` always releasable; short-lived feature branches named `<type>/<id>-<slug>` (for example `feat/FR-ART-003-create-article`); merge after checks pass.
- Commits [RECOMMENDATION]: Conventional Commits style (`feat:`, `fix:`, `docs:`, `test:`, `chore:`) with requirement or issue IDs in the message.
- The RealWorld reference is a Git submodule of `https://github.com/realworld-apps/realworld`, pinned to a recorded commit (DEC-006 b, APPROVED). Path `realworld-main/` and commit `25588846` are APPROVED (DEC-012, E-09). It stays unmodified. Before replacing the downloaded `realworld-main/` folder, compare `specs/` file hashes and report differences.
- Do not commit `swebok-v4.pdf` (IEEE copyright) or `_archive/` (DEC-008, APPROVED). NASA reference PDFs are committed. The other `.gitignore` and `.gitattributes` rules are APPROVED (DEC-011, E-09).
- Never commit secrets, `.env` files, virtual environments, `node_modules`, build output, or test reports containing credentials.

## 8. Testing Tools

| Level | Tool | Status |
| --- | --- | --- |
| Backend unit and integration | pytest; a Django integration plugin is a Phase 2 decision (not yet proposed) | pytest: Agreed |
| API conformance | RealWorld Hurl suite (`realworld-main/specs/api/hurl/`) run by Hurl for Windows | Approved (DEC-004); not yet installed |
| Frontend E2E | Playwright with the RealWorld suite (`realworld-main/specs/e2e/`) | Agreed |
| Frontend unit | Vitest | Approved (DEC-003) |

Notes:
- The RealWorld runner scripts (`run-api-tests-hurl.sh`, `run-hurl-tests.sh`) require bash. Hurl is run directly from PowerShell with equivalent `--test --jobs 1 --variable host=... --variable uid=...` arguments (DEC-004). Hurl is installed at the start of Phase 3 or Phase 4.

## 9. Environment Configuration

- All environment-specific values (database URL or credentials, secret key, JWT settings, allowed origins, debug flag, API base URL) come from environment variables.
- Commit an example file (for example `.env.example`) with placeholder values only. Real values stay in uncommitted local files or the deployment environment.
- Debug mode is off in any deployed environment.

## 10. Dependency Management

- Backend: Python built-in `venv` plus `pip`, with pinned versions in a committed requirements file (DEC-005). No additional tool (Poetry, uv, pipenv) without approval.
- Frontend (DEC-013, APPROVED): Node.js with `npm` and a committed `package-lock.json`.
- Add a dependency only when an approved design needs it; record why in the decision record or design. Open-source components are evaluated before adoption (licence, maintenance status, known vulnerabilities), in the spirit of NPR 7150.2D SWE-027.
- Check new and existing dependencies for known vulnerabilities before each gate from QG3 onward with `pip-audit` (DEC-005, APPROVED) and `npm audit` (DEC-013, APPROVED).
- Note: the RealWorld repository itself uses `bun` for its own docs tooling. That convention applies only to that repository and is not part of this project's stack.

## 11. Coding Conventions

General:
- Code, identifiers, comments, and commit messages in English. Project documents are bilingual, English and Vietnamese (DEC-016, APPROVED in principle; format details are open, see `docs/00-governance/project-plan.md` §9 OPEN-01). Existing governance documents keep their current language until OPEN-01 is decided.
- Small, single-purpose functions and modules. No dead code, no commented-out code in commits.
- Reference requirement IDs in tests (test names or docstrings) to support traceability.
- The coding standard and secure coding practices are recorded in Phase 2 (NPR 7150.2D SWE-061, SWE-207) and checked with static analysis in Phase 3 (SWE-135, SWE-185).

Python:
- PEP 8 style; type hints on public functions; Django Ninja schemas for request/response validation.
- Formatter and linter: Ruff (DEC-005).

JavaScript/React:
- Function components and hooks.
- Linter: ESLint; formatter: Prettier (DEC-005).

## 12. Open Technology Decisions (for Phase 2 unless the user decides earlier)

- Django, Django Ninja, React, Vite, Playwright, Vitest, Hurl, Ruff, ESLint, Prettier, and pip-audit versions.
- JavaScript or TypeScript for the frontend.
- Node.js version (toolchain approved in DEC-013; version recorded in Phase 2).
- JWT library, Markdown renderer, router library, HTTP client approach.
- Ports and local URLs (ISS-002).
- Deployment target and method (Phase 5 approach decided in Phase 2).
