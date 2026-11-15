# Decision Log

Project: Software Engineering Practice with RealWorld Conduit
Format: see `.kiro/steering/engineering-process.md` §4.
Approver for all decisions: the user (project owner).

| ID | Title | Status | Date |
| --- | --- | --- | --- |
| DEC-001 | Governance baseline (steering v1.0) | APPROVED | 2026-11-16 |
| DEC-002 | Lifecycle documentation location | APPROVED | 2026-11-16 |
| DEC-003 | Frontend unit test tool: Vitest | APPROVED | 2026-11-16 |
| DEC-004 | Hurl on Windows, run from PowerShell | APPROVED | 2026-11-16 |
| DEC-005 | Code quality tools and Python dependency management | APPROVED | 2026-11-16 |
| DEC-006 | Repository layout | APPROVED | 2026-11-16 |
| DEC-007 | RealWorld ecosystem expectations | APPROVED | 2026-11-16 |
| DEC-008 | Repository initialization and published content | APPROVED (items marked as agent choices await confirmation) | 2026-11-16 |

---

## DEC-001: Governance baseline (steering v1.0)

- Context: The project needs a governance baseline before Phase 1.
- Options: Accept the five steering files; revise them first.
- Decision: Accept `.kiro/steering/` v1.0 (`project-overview.md`, `engineering-process.md`, `requirements-and-sources.md`, `technology-stack.md`, `quality-assurance.md`).
- Rationale: Covers lifecycle, sources, stack, and quality rules required by the user's task.
- Consequences: Changes to the steering files follow change control (`requirements-and-sources.md` §6). The v1.1 additions (verified NASA/SWEBOK references, ISS-007 to ISS-009) are not covered by this decision and await confirmation.
- Status: APPROVED by the user, 2026-11-16.

## DEC-002: Lifecycle documentation location

- Context: Lifecycle documents need a fixed, version-controlled location.
- Options: `docs/` with one folder per phase; a single document; an external tool.
- Decision: `docs/00-governance/`, `docs/01-requirements/`, `docs/02-design/`, `docs/03-construction/`, `docs/04-testing/`, `docs/05-deployment/` at the repository root.
- Rationale: Simple, kept with the code under Git, maps one-to-one to the phases.
- Consequences: Each phase's evidence and gate record lives in its folder. Assumption A-06: the repository root is the workspace root `E:\2026-11-16-SoftwareEngineering` (to be confirmed).
- Status: APPROVED by the user, 2026-11-16.

## DEC-003: Frontend unit test tool: Vitest

- Context: ISS-005. RealWorld expects at least one unit test per repository; Playwright is mainly an E2E tool.
- Options: (A) Vitest; (B) Playwright component testing (experimental); (C) E2E only (does not meet the RealWorld expectation).
- Decision: Option A, Vitest, added to the stack for frontend unit tests. Playwright remains the E2E tool.
- Rationale: Native Vite integration, minimal configuration, stable.
- Consequences: Stack addition. Version pinned in Phase 2. ISS-005 is resolved.
- Status: APPROVED by the user, 2026-11-16.

## DEC-004: Hurl on Windows, run from PowerShell

- Context: The RealWorld API suite uses Hurl. Hurl is not installed; the RealWorld runner scripts need bash, which is not available.
- Options: Install Hurl for Windows and run it from PowerShell; install bash (Git Bash or WSL2); skip the Hurl suite.
- Decision: Install Hurl for Windows and run it directly from PowerShell with arguments equivalent to `run-hurl-tests.sh` (`--test --jobs 1 --variable host=... --variable uid=...`). Install at the start of Phase 3 or Phase 4.
- Rationale: Keeps the official conformance suite without bash or WSL2 (WSL2 is deferred).
- Consequences: A PowerShell runner script is written in Phase 3. The Hurl version is recorded with test evidence.
- Status: APPROVED by the user, 2026-11-16.

## DEC-005: Code quality tools and Python dependency management

- Context: Static analysis is needed from Phase 3 (NPR 7150.2D SWE-135, SWE-185). Python dependency management was unspecified.
- Options: Ruff, ESLint, Prettier, pip-audit, venv + pip; alternatives such as flake8/black, Poetry, or uv.
- Decision: Adopt Ruff (Python lint and format), ESLint (JavaScript lint), Prettier (frontend format), pip-audit (Python dependency vulnerability scan; `npm audit` for npm), and Python `venv` + `pip` with a pinned requirements file.
- Rationale: Few tools, standard for each ecosystem, no extra package manager.
- Consequences: Stack additions. Versions pinned in Phase 2/3. Poetry, uv, and pipenv are not used.
- Status: APPROVED by the user, 2026-11-16.

## DEC-006: Repository layout

- Context: Repository structure and storage of the RealWorld reference were undecided.
- Decision:
  - (a) One repository (monorepo) for backend, frontend, and documentation.
  - (b) The RealWorld reference is a Git submodule pointing to the official repository `https://github.com/realworld-apps/realworld`, pinned to a specific commit.
  - (c) GitHub repository name `2026-11-16_NDX_SoftwareEngineering`, visibility public (decided by the user, 2026-11-16).
- Rationale: (a) simpler traceability for one developer; (b) records the exact upstream commit, resolving ISS-001.
- Consequences:
  - The current downloaded `realworld-main/` folder has no Git metadata. When the repository is created, the submodule replaces it. Recommended path: keep `realworld-main/` so existing references stay valid.
  - The upstream commit chosen for the submodule may differ from the downloaded copy. File hashes of `specs/` must be compared before switching; differences are reported to the user.
  - Repository initialization: immediately, before Phase 1 (DEC-008).
  - Result: the downloaded copy matched upstream commit `25588846` exactly; the submodule is pinned there (`docs/00-governance/realworld-baseline.md`). ISS-001 is resolved.
- Status: APPROVED by the user, 2026-11-16 (part c added the same day).

## DEC-007: RealWorld ecosystem expectations

- Context: ISS-006. `expectations.md` lists expectations for published implementations.
- Decision:
  - A README explaining how to run the application locally becomes an engineering constraint (CON).
  - Public repository: yes (DEC-006 c). Enabling the GitHub Issues section and keeping the implementation up to date after the project: not yet confirmed by the user.
  - The ≥300 GitHub stars criterion for Django, Django Ninja, React, and Vite is verified in Phase 2 when versions are chosen.
- Rationale: The README supports reproducibility (QO-5); the other items concern publishing, not the learning objectives.
- Consequences: ISS-006 is resolved except the Issues section and post-project maintenance, which await user confirmation.
- Status: APPROVED by the user, 2026-11-16.

## DEC-008: Repository initialization and published content

- Context: DEC-006 approved a public monorepo with a RealWorld submodule. The user decided to initialize Git immediately, confirmed the repository root (A-06), and decided which reference PDFs to commit.
- Decision (user):
  - Repository root: `E:\2026-11-16-SoftwareEngineering`, branch `main`, initialized now, before Phase 1.
  - Commit the NASA reference PDFs (`The NASA Software Engineering Handbook/`).
  - Do not commit `swebok-v4.pdf` (IEEE copyright); it is listed in `.gitignore` and stays local.
- Implementation choices by the agent (await user confirmation):
  - The downloaded RealWorld copy was moved to `_archive/realworld-main-download-2026-11-16/` (local only, ignored by Git) before adding the submodule at `realworld-main/`.
  - `RealWorld apps Documentation.docx` exists only in the downloaded copy, so it stays in `_archive/` and is not committed. Publishing it was not decided by the user.
  - The 2026-11-15 note (`2026-11-15 at 11.39.44 Note.txt`) is committed as the record of the agreed objectives. It contains the old OneDrive path.
  - `.gitattributes` normalizes text files to LF in the repository.
  - Nothing is pushed to GitHub. The user creates the GitHub repository and performs the first push.
- Consequences: All governance artifacts are under version control from the first commit. The RealWorld baseline is recorded in `docs/00-governance/realworld-baseline.md`.
- Status: APPROVED by the user, 2026-11-16 (user decisions). Agent implementation choices: PROPOSED.

---

## Open points

1. Enable the GitHub Issues section, and keep the implementation updated after the project (DEC-007).
2. Confirm the DEC-008 implementation choices (archive location, `.docx` not committed, note file committed).
3. Confirm the steering v1.1 additions and the list of NASA requirements proposed as not applicable (`engineering-process.md` §11).
4. Decide whether to do the optional learning exercises: software classification (SWE-020) and requirements mapping matrix (SWE-125).
