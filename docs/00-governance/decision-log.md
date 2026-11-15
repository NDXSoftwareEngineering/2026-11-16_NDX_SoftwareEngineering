# Decision Log

Project: Software Engineering Practice with RealWorld Conduit
Record format: `.kiro/steering/engineering-process.md` §4. Statuses: `engineering-process.md` §2.
Approver for all decisions: the user (project owner). Kiro never records approval without an entry in the evidence register (§1).
Log revision: 2 (2026-11-16, Quality Gate S1 corrections SG-01/SG-02). Revision 1 marked several agent-proposed items as approved; those items are now separated and marked PROPOSED or PENDING USER APPROVAL.
Log revision: 3 (2026-11-16). Records the user's Quality Gate S1 decision (evidence E-09): DEC-011 to DEC-014 approved; steering v1.4 approved.

## 1. Approval Evidence Register

All evidence below is user messages in the Kiro chat session of 2026-11-16, transcribed verbatim by Kiro. Each quote was written in reply to the Kiro message named in "Context".

Limitation (observation OBS-S1-01): the chat transcript is not stored in this repository, so a reviewer cannot check these quotes against an independent record. The user's acceptance of Quality Gate S1 would also confirm this register.

| ID | Verbatim user statement | Context (what the user was answering) | Explicitness |
| --- | --- | --- | --- |
| E-01 | "1. Tôi phê duyệt bộ Steering. 2. Tôi đã cài pypdf cho bạn" | Kiro's question whether to approve the five steering files (v1.0) and whether to read the PDFs | Explicit |
| E-02 | "Duyệt 1,2,3,4,5,6,7" | Kiro's bilingual list of decisions 1–7, each with a stated recommendation (3: option A Vitest; 6: a monorepo, b submodule, c user's choice; 7: README as constraint, rest depends on 6c) | Explicit for the recommendations as presented; no option letters restated by the user |
| E-03 | "Quyết định 6(c): tên repository trên GitHub là 2026-11-16_NDX_SoftwareEngineering, và để công khai (public) Giả định A-06: gốc repository là E:\2026-11-16-SoftwareEngineering, đúng. Thời điểm khởi tạo Git và submodule: làm ngay. Có commit các file PDF tham khảo không: chỉ commit tài liệu của NASA. Còn SWEBOK thì không nên" | Kiro's open points: repository name and visibility, A-06, timing of `git init`, which PDFs to commit | Explicit |
| E-04 | "Tài khoản Github của tôi là NDXSoftwareEngineering. Tôi đã tạo một Repository có tên là 2026-11-16_NDX_SoftwareEngineering ở chế độ Public" | Kiro's offer to run `git remote add` and `git push` once given the account name | Implied instruction to push; not a stated approval |
| E-05 | "Có bật mục Issues trên GitHub, Có tiếp tục cập nhật ứng dụng sau khi dự án kết thúc không (DEC-007)." | Kiro's two questions: enable Issues; keep the application updated after the project | Explicit for Issues. Ambiguous for maintenance (repeats the question form "Có … không") |
| E-06 | "Xác nhận các lựa chọn tôi tự đưa ra khi làm DEC-008: để bản tải về trong _archive/, không commit file .docx, đã commit file ghi chú. tôi xác nhận" | Kiro's request to confirm three listed DEC-008 implementation choices | Explicit for the three listed items only |
| E-07 | "Bài tập tùy chọn: không làm 2 bài tập đó ngay." | Kiro's question whether to do the SWE-020 and SWE-125 exercises | Explicit |
| E-08 | "Chấp nhận phần bổ sung v1.1, với giới hạn đã nêu ở mục 1. Chấp nhận danh sách "không áp dụng" gồm SWE-032, SWE-070, IV&V (SWE-141 và các yêu cầu liên quan) và SWE-023/134/184/192. Chuyển SWE-136 sang "áp dụng rút gọn", và thêm ghi chú về SWE-143. Tôi chấp nhận" | Kiro's three proposals on v1.1 references and NASA applicability | Explicit |
| E-09 | "1. Chấp nhận 2. Xác nhận. 3. Duyệt cả 3. 4. Có. 5. Tôi không có nội dung gì" | Kiro's five Quality Gate S1 decision items: (1) accept S1, (2) confirm the evidence register E-01 to E-08, (3) approve DEC-011, DEC-012, DEC-013, (4) DEC-014 maintenance yes/no, (5) content of SG-06 | Explicit for all five items |

## 2. Decision Index

| ID | Title | Status | Approver | Date | Evidence |
| --- | --- | --- | --- | --- | --- |
| DEC-001 | Steering baseline v1.0 | APPROVED (v1.0 only) | User | 2026-11-16 | E-01 |
| DEC-002 | Lifecycle documentation location | APPROVED | User | 2026-11-16 | E-02 |
| DEC-003 | Frontend unit test tool: Vitest | APPROVED | User | 2026-11-16 | E-02 |
| DEC-004 | Hurl for Windows, run from PowerShell | APPROVED | User | 2026-11-16 | E-02 |
| DEC-005 | Code quality tools and Python dependency management | APPROVED | User | 2026-11-16 | E-02 |
| DEC-006 | Repository layout (monorepo, submodule, name, visibility) | APPROVED | User | 2026-11-16 | E-02, E-03 |
| DEC-007 | RealWorld ecosystem expectations (README, public, Issues) | APPROVED | User | 2026-11-16 | E-02, E-03, E-05 |
| DEC-008 | Repository initialization and published content | APPROVED | User | 2026-11-16 | E-03, E-06; push: E-04 (implied) |
| DEC-009 | Steering v1.1 references and NASA applicability | APPROVED | User | 2026-11-16 | E-08 |
| DEC-010 | Defer SWE-020 and SWE-125 learning exercises | APPROVED | User | 2026-11-16 | E-07 |
| DEC-011 | `.gitattributes` and `.gitignore` contents | APPROVED | User | 2026-11-16 | E-09 |
| DEC-012 | Submodule path and pinned commit | APPROVED | User | 2026-11-16 | E-09 |
| DEC-013 | Node.js runtime and npm for the frontend | APPROVED | User | 2026-11-16 | E-09 |
| DEC-014 | Post-project maintenance of the implementation: yes | APPROVED | User | 2026-11-16 | E-09 (E-05 was ambiguous) |

DEC-011 to DEC-014 were split out of DEC-006, DEC-007, and DEC-008 (revision 1), or were stated in the steering files as rules without approval (DEC-013).

## 3. Decision Records

### DEC-001: Steering baseline v1.0

- Context: The project needed a governance baseline before Phase 1.
- Options: Accept the five steering files; revise them first.
- Decision: Accept `.kiro/steering/` v1.0 (`project-overview.md`, `engineering-process.md`, `requirements-and-sources.md`, `technology-stack.md`, `quality-assurance.md`).
- Rationale: Covers lifecycle, sources, stack, and quality rules required by the user's task.
- Consequences: Later versions are not covered by DEC-001; see §4 Steering approval history.
- Observation OBS-S1-02: Git was initialized at steering v1.2, so the exact v1.0 text that the user approved is not preserved in the repository.
- Status: APPROVED for v1.0 only. Evidence: E-01.

### DEC-002: Lifecycle documentation location

- Context: Lifecycle documents need a fixed, version-controlled location.
- Options: `docs/` with one folder per phase; a single document; an external tool.
- Decision: `docs/00-governance/`, `docs/01-requirements/`, `docs/02-design/`, `docs/03-construction/`, `docs/04-testing/`, `docs/05-deployment/` at the repository root.
- Rationale: Simple, kept with the code under Git, maps one-to-one to the phases.
- Consequences: Each phase's evidence and gate record lives in its folder. The repository root is `E:\2026-11-16-SoftwareEngineering` (A-06, evidence E-03).
- Agent detail (not separately approved, low impact): each phase folder is created when its phase starts.
- Status: APPROVED. Evidence: E-02 (item 2).

### DEC-003: Frontend unit test tool: Vitest

- Context: ISS-005. RealWorld expects at least one unit test per repository; Playwright is mainly an E2E tool.
- Options: (A) Vitest; (B) Playwright component testing (experimental); (C) E2E only (does not meet the RealWorld expectation).
- Decision: Option A, Vitest, for frontend unit tests. Playwright remains the E2E tool.
- Rationale: Native Vite integration, minimal configuration.
- Consequences: Stack addition. Version pinned in Phase 2. ISS-005 is resolved.
- Status: APPROVED. Evidence: E-02 (item 3, recommendation A as presented).

### DEC-004: Hurl for Windows, run from PowerShell

- Context: The RealWorld API suite uses Hurl. Hurl is not installed; the RealWorld runner scripts need bash, which is not available.
- Options: Install Hurl for Windows and run it from PowerShell; install bash (Git Bash or WSL2); skip the Hurl suite.
- Decision: Install Hurl for Windows and run it directly from PowerShell with arguments equivalent to `run-hurl-tests.sh` (`--test --jobs 1 --variable host=... --variable uid=...`). Install at the start of Phase 3 or Phase 4.
- Rationale: Keeps the official conformance suite without bash or WSL2 (WSL2 is deferred).
- Consequences: A PowerShell runner script is written in Phase 3. The Hurl version is recorded with test evidence.
- Status: APPROVED. Evidence: E-02 (item 4).

### DEC-005: Code quality tools and Python dependency management

- Context: Static analysis is needed from Phase 3 (NPR 7150.2D SWE-135, SWE-185). Python dependency management was unspecified.
- Options: Ruff, ESLint, Prettier, pip-audit, venv + pip; alternatives such as flake8/black, Poetry, or uv.
- Decision: Adopt Ruff (Python lint and format), ESLint (JavaScript lint), Prettier (frontend format), pip-audit (Python dependency vulnerability scan), and Python `venv` + `pip` with a pinned requirements file.
- Rationale: Few tools, standard for each ecosystem, no extra package manager.
- Consequences: Stack additions. Versions pinned in Phase 2/3. Poetry, uv, and pipenv are not used. `npm audit` is covered by DEC-013 (APPROVED, E-09).
- Status: APPROVED. Evidence: E-02 (item 5).

### DEC-006: Repository layout

- Context: Repository structure and storage of the RealWorld reference were undecided.
- Decision:
  - (a) One repository (monorepo) for backend, frontend, and documentation. Evidence: E-02.
  - (b) The RealWorld reference is a Git submodule of `https://github.com/realworld-apps/realworld`, pinned to a specific commit. Evidence: E-02.
  - (c) GitHub repository `2026-11-16_NDX_SoftwareEngineering`, public. Evidence: E-03.
- Rationale: (a) simpler traceability for one developer; (b) records the exact upstream commit, resolving ISS-001.
- Consequences: The submodule path and the commit chosen are implementation choices recorded separately in DEC-012 (APPROVED, E-09).
- Status: APPROVED. Evidence: E-02 (item 6 a, b), E-03 (6 c).

### DEC-007: RealWorld ecosystem expectations

- Context: ISS-006. RealWorld `expectations.md` lists expectations for published implementations.
- Decision:
  - A README explaining how to run the application locally is engineering constraint C-07. Evidence: E-02.
  - Public repository. Evidence: E-03.
  - GitHub Issues section enabled. Evidence: E-05. Verified 2026-11-16 via the GitHub REST API (`GET /repos/NDXSoftwareEngineering/2026-11-16_NDX_SoftwareEngineering`): `"has_issues": true`.
  - The ≥300 GitHub stars criterion for Django, Django Ninja, React, and Vite is checked in Phase 2. Evidence: E-02.
- Consequences: Post-project maintenance is not covered; see DEC-014.
- Status: APPROVED. Evidence: E-02 (item 7), E-03, E-05.

### DEC-008: Repository initialization and published content

- Context: DEC-006 approved a public monorepo with a RealWorld submodule.
- Decision (user, evidence E-03):
  - Repository root `E:\2026-11-16-SoftwareEngineering`; initialize Git and the submodule immediately, before Phase 1.
  - Commit the NASA reference PDFs (`The NASA Software Engineering Handbook/`).
  - Do not commit `swebok-v4.pdf`.
- Implementation choices confirmed by the user (evidence E-06):
  - The downloaded RealWorld copy is kept locally in `_archive/realworld-main-download-2026-11-16/` and not committed.
  - `RealWorld apps Documentation.docx` is not committed.
  - The note `2026-11-15 at 11.39.44 Note.txt` is committed.
- Action: Kiro pushed `main` to `origin` on 2026-11-16 after the user created the repository and gave the account name (evidence E-04, implied instruction; observation OBS-S1-03).
- Not covered by E-06: `.gitattributes` and `.gitignore` contents (now DEC-011, APPROVED with evidence E-09).
- Status: APPROVED. Evidence: E-03, E-06; push E-04.

### DEC-009: Steering v1.1 references and NASA applicability

- Context: Steering v1.1 added NASA and SWEBOK references from the local PDFs (text extracted with pypdf). Several NPR 7150.2D requirements were proposed as not applicable.
- Decision:
  - Accept the v1.1 additions, with these limits: the phase-to-SWE and phase-to-SWEBOK mapping is Kiro's engineering judgment; NPR 7150.2D SWE-052 Table 1 is not cited per software class because its columns were lost in text extraction.
  - Not applicable: SWE-032, SWE-070, SWE-141 and related IV&V requirements, SWE-023, SWE-134, SWE-184, SWE-192. Kiro's reading of "related IV&V requirements" is SWE-131, SWE-178, SWE-179 (listed by Kiro, not by the user).
  - SWE-136 applied in reduced form: record tool versions in evidence, use stable maintained releases, pin versions; no formal accreditation.
  - SWE-143 note: required only for Category 1/2 projects; the QG2 architecture review is a project choice.
- Status: APPROVED. Evidence: E-08.

### DEC-010: Defer SWE-020 and SWE-125 learning exercises

- Context: Optional exercises proposed in `engineering-process.md` §11.
- Decision: Do not do the software classification (SWE-020) or requirements mapping matrix (SWE-125) exercises now. They may be revisited.
- Status: APPROVED. Evidence: E-07.

### DEC-011: `.gitattributes` and `.gitignore` contents

- Context: Created by Kiro at repository initialization (commit `0a54008`). Only the exclusion of `swebok-v4.pdf` (E-03) and of `_archive/` (E-06) has user evidence.
- Proposal: Keep `.gitattributes` (text normalized to LF in the repository; `*.ps1` checked out with CRLF; PDF, DOCX, PNG as binary) and the `.gitignore` patterns for secrets (`.env`, `.env.*` except `.env.example`), Python and Node build artifacts, test reports, and OS/editor files (`Thumbs.db`, `Desktop.ini`, `.DS_Store`, `.vscode/`).
- Rationale: Prevents committing secrets and generated files; gives consistent line endings on Windows.
- Status: APPROVED. Evidence: E-09 (item 3).

### DEC-012: Submodule path and pinned commit

- Context: DEC-006 (b) approved a pinned submodule but not its path or commit.
- Proposal: Path `realworld-main/` (keeps existing references valid); pinned commit `25588846c314b15a5e427ffe816e6b4af7be68c7` (upstream HEAD on 2026-11-16, identical to the downloaded copy that was inspected; see `realworld-baseline.md`).
- Status: APPROVED. Evidence: E-09 (item 3).

### DEC-013: Node.js runtime and npm for the frontend

- Context: Vite, React, Vitest, ESLint, Prettier, and Playwright run on Node.js, but Node.js and a package manager are not named in the agreed stack. Steering v1.3 stated "Frontend: `npm` with a committed `package-lock.json`" as a rule without approval.
- Proposal: Use the installed Node.js (observed v24.22.3) with npm and a committed `package-lock.json`; use `npm audit` for dependency scanning. Exact Node.js version recorded in Phase 2.
- Alternatives: pnpm, Yarn, or Bun (each would add a tool).
- Status: APPROVED. Evidence: E-09 (item 3).

### DEC-014: Post-project maintenance of the implementation

- Context: RealWorld `expectations.md` asks implementers to keep implementations up to date. Evidence E-05 repeats the question form and does not state a choice.
- Options: Yes, maintain after QG5; No, the repository is frozen after QG5.
- Decision: Yes. The implementation is kept up to date after QG5.
- Consequences: Maintenance scope and frequency are not yet defined. They are to be planned in Phase 5 (NPR 7150.2D SWE-075 operations and maintenance planning, as mapped in `engineering-process.md` §11). A steering change for this will be proposed when Phase 5 is planned.
- Status: APPROVED. Evidence: E-09 (item 4).

## 4. Steering Approval History

| Version | Content | Status | Evidence |
| --- | --- | --- | --- |
| 1.0 | Initial five steering files | APPROVED | E-01. Text not preserved in Git (OBS-S1-02). |
| 1.1 | NASA and SWEBOK references, ISS-007 to ISS-009 | Content APPROVED | E-08 (DEC-009) |
| 1.2 | Text edits applying DEC-002 to DEC-008 | Decisions approved; the edited text itself was not reviewed by the user | E-02, E-03, E-06 cover decisions only |
| 1.3 | Text edits applying DEC-009 and DEC-010; status line "APPROVED by the user" | Decisions approved; edited text not reviewed. The status line overstated approval (SG-05). | E-07, E-08 cover decisions only |
| 1.4 | Quality Gate S1 corrections (SG-01 to SG-05) | APPROVED with Quality Gate S1 acceptance | E-09 (item 1) |
| 1.4, status update | After E-09, only status words and evidence IDs were changed to record DEC-011 to DEC-014 and ISS-010; no other text changed. Commit message: "docs: record Quality Gate S1 acceptance (E-09)". | Administrative record of E-09 | E-09 |

## 5. Open Points

1. None of DEC-001 to DEC-014 is open.
2. Quality Gate S1: ACCEPTED by the user on 2026-11-16 (E-09). Gate record: `docs/00-governance/qg-s1-review-package.md` §7.
3. Issues carried forward with the S1 acceptance: ISS-002, ISS-003, ISS-004, ISS-007, ISS-008, ISS-009 (`requirements-and-sources.md` §8).
