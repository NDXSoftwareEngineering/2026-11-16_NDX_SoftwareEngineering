---
inclusion: always
---

# Technology Stack

Baseline version: 1.2 (2026-11-16). Status: v1.0 APPROVED by the user on 2026-11-16 (DEC-001). v1.1 added NASA and SWEBOK references verified from the local PDFs; these await user confirmation. v1.2 applies approved decisions DEC-002 to DEC-008 (`docs/00-governance/decision-log.md`).

## 1. Change Policy

The agreed stack is: Windows 11, Kiro, Python 3.12, Django, Django Ninja, React, Vite, PostgreSQL 17, pytest, Playwright, Git, GitHub.

Approved additions (see `docs/00-governance/decision-log.md`):
- Vitest for frontend unit tests (DEC-003).
- Hurl for Windows, run from PowerShell (DEC-004).
- Ruff, ESLint, Prettier, pip-audit, and Python `venv` + `pip` (DEC-005).

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
| Workspace | `E:\2026-11-16-SoftwareEngineering` is the Git repository root, branch `main` (DEC-008). Not yet pushed to GitHub. |

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
- The RealWorld reference is a Git submodule of `https://github.com/realworld-apps/realworld`, pinned to a recorded commit (DEC-006 b). It stays unmodified. Before replacing the downloaded `realworld-main/` folder, compare `specs/` file hashes and report differences.
- Do not commit `swebok-v4.pdf` (IEEE copyright) or `_archive/` (DEC-008). NASA reference PDFs are committed.
- Never commit secrets, `.env` files, virtual environments, `node_modules`, build output, or test reports containing credentials.

## 8. Testing Tools

| Level | Tool | Status |
| --- | --- | --- |
| Backend unit and integration | pytest (with a Django integration plugin, chosen in Phase 2) | Agreed |
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
- Frontend: `npm` with a committed `package-lock.json`.
- Add a dependency only when an approved design needs it; record why in the decision record or design. Open-source components are evaluated before adoption (licence, maintenance status, known vulnerabilities), in the spirit of NPR 7150.2D SWE-027.
- Check new and existing dependencies for known vulnerabilities before each gate from QG3 onward with `pip-audit` (DEC-005) and `npm audit`.
- Note: the RealWorld repository itself uses `bun` for its own docs tooling. That convention applies only to that repository and is not part of this project's stack.

## 11. Coding Conventions

General:
- Code, identifiers, comments, and commit messages in English. Project documents may be in English or Vietnamese as the user chooses, but one language per document.
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
- JWT library, Markdown renderer, router library, HTTP client approach.
- Ports and local URLs (ISS-002).
- Deployment target and method (Phase 5 approach decided in Phase 2).
