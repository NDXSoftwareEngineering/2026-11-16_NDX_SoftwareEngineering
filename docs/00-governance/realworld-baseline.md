# RealWorld Reference Baseline

Date: 2026-11-16. Related: ISS-001, DEC-006 (b), DEC-008.

## Baseline

| Item | Value |
| --- | --- |
| Upstream repository | https://github.com/realworld-apps/realworld |
| Pinned commit | `25588846c314b15a5e427ffe816e6b4af7be68c7` |
| Commit date | 2026-11-02 08:06:00 +0100 |
| Commit message | `fix: right navbar selector in auth thest for /login` |
| Submodule path | `realworld-main/` |
| OpenAPI version in this commit | `info.version: 2.0.0` (`specs/api/openapi.yml`) |

## Verification of the downloaded copy

Method: for every file in the downloaded copy (now in `_archive/realworld-main-download-2026-11-16/`, local only), the Git blob SHA-1 of the raw bytes was computed and compared with the blob IDs from `git ls-tree -r` for each upstream commit, starting at HEAD.

Result (from script output on 2026-11-16):

| Measure | Value |
| --- | --- |
| Files in downloaded copy | 338 |
| Files compared (excluding the `.docx`) | 337 |
| Files in upstream tree at the pinned commit | 337 |
| Files with identical content | 337 |
| Files only upstream / only local / content differs | 0 / 0 / 0 |
| Of which under `specs/` | 201 |

Conclusion: the downloaded copy is identical to upstream commit `25588846`. Everything inspected during governance setup applies to the pinned submodule. ISS-001 is resolved.

## File not in the upstream repository

`RealWorld apps Documentation.docx` was present only in the downloaded copy. It is not part of the upstream repository. It is kept locally at `_archive/realworld-main-download-2026-11-16/RealWorld apps Documentation.docx` and is not committed (see DEC-008). It remains source SRC-DOCX with the lowest precedence (ISS-003).

## Updating the baseline

Moving the submodule to another commit is a change to a baselined configuration item. It requires a change request (`CR-NNN`), a comparison of `specs/` against the current pin, and user approval.
