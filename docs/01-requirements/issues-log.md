# Phase 1 Issues Log / Nhật ký vấn đề Giai đoạn 1

Status / Trạng thái: **DRAFT** (increment 1, 2026-11-18). Author: Kiro. Decision authority: the user.
Format: `.kiro/steering/requirements-and-sources.md` §7. Baseline issues ISS-001 to ISS-010 are in that file §8; this log continues from ISS-011.
Language: English, with a Vietnamese summary per issue. The bilingual format is not final (project-plan OPEN-01).

Vietnamese summary / Tóm tắt: Khi đọc nguồn RealWorld, tôi tìm thấy 10 điểm mơ hồ, mâu thuẫn hoặc chưa kiểm chứng được. Mỗi điểm dưới đây có nguồn dẫn chứng, đề xuất và trạng thái. Không điểm nào được tự giải quyết; mọi quyết định thuộc về người dùng.

Source precedence used for recommendations (steering `requirements-and-sources.md` §1): SRC-API (OpenAPI) and SRC-HURL, then SRC-E2E, then SRC-DOC, then SRC-DOCX. RealWorld states that tests win over prose (`backend/introduction.md`). Recommendations are not decisions.

| ID | Type | Status |
| --- | --- | --- |
| ISS-011 | CONFLICT | OPEN |
| ISS-012 | CONFLICT | OPEN |
| ISS-013 | AMBIGUOUS | OPEN |
| ISS-014 | AMBIGUOUS | OPEN |
| ISS-015 | UNVERIFIABLE | OPEN |
| ISS-016 | UNVERIFIABLE | OPEN |
| ISS-017 | MISSING | OPEN |
| ISS-018 | CONFLICT | OPEN |
| ISS-019 | AMBIGUOUS | OPEN (minor) |
| ISS-020 | UNVERIFIABLE | OPEN, waiting for a source to be read |

## ISS-011: Status code for duplicate username or email

- Type: CONFLICT (prose versus tests).
- Sources:
  - SRC-API `openapi.yml`: `POST /users` declares response `409` (`ConflictError`, example `errors.username: ["has already been taken"]`).
  - SRC-HURL `errors_auth.hurl`: duplicate username and duplicate email each assert `HTTP 409` with `$.errors.username[0] == "has already been taken"` and `$.errors.email[0] == "has already been taken"`.
  - SRC-DOC `backend/error-handling.md`: states that validation failures return 422, and lists the message `has already been taken` in the table of messages under that 422 text. It does not state a status code for duplicates explicitly.
- Impact: The wrong status would fail 2 Hurl requests and could break the frontend error display.
- Options: (A) 409 for duplicates, 422 for other validation errors. (B) 422 for everything.
- Recommendation: A, because OpenAPI and Hurl agree and take precedence over prose.
- Vietnamese: Tài liệu `error-handling.md` dễ hiểu là 422 cho trùng lặp, nhưng OpenAPI và Hurl đều dùng 409. Đề xuất theo Hurl (409).
- Status: OPEN.

## ISS-012: 409 declared for article creation, but duplicate titles are allowed

- Type: CONFLICT (OpenAPI versus Hurl).
- Sources:
  - SRC-API: `POST /articles` declares `409` (`ConflictError`).
  - SRC-HURL `errors_articles.hurl`: "Duplicate titles are allowed (each gets a unique slug)": two articles with the same title both return `HTTP 201` with different slugs.
  - SRC-DOC `endpoints.md`: the slug only needs to be unique; duplicate titles must produce distinct slugs.
  - No Hurl request in the suite produces a 409 on `POST /articles`.
- Impact: The condition that produces 409 on article creation is undefined.
- Options: (A) Never return 409 on article creation. (B) Return 409 only if a unique slug cannot be generated.
- Recommendation: A, because no source defines another trigger. Verification: a project test, not Hurl.
- Vietnamese: OpenAPI khai báo 409 cho tạo bài viết, nhưng Hurl cho phép trùng tiêu đề. Không nguồn nào nói khi nào trả 409.
- Status: OPEN.

## ISS-013: Optional-auth endpoints declare 401, behavior with an invalid token is undefined

- Type: AMBIGUOUS.
- Sources:
  - SRC-API: `GET /profiles/{username}`, `GET /articles`, `GET /articles/{slug}/comments` are described as "Auth is optional" and also declare `401`. `GET /articles/{slug}` and `GET /tags` ("Auth not required") do not declare 401.
  - SRC-HURL: no request on these endpoints sends an invalid or expired token; the 401 responses on these three operations are never asserted.
  - SRC-DOC `endpoints.md`: "Authentication optional" without stating what happens with a bad token.
- Impact: Backend behavior with an invalid token on optional-auth endpoints cannot be verified from the sources.
- Options: (A) Treat an invalid token as 401. (B) Ignore an invalid token and treat the request as anonymous.
- Recommendation: A, because OpenAPI declares 401 on those operations. Mark the requirement as verified by a project test only.
- Vietnamese: Các endpoint "xác thực tùy chọn" vẫn khai báo 401, nhưng không có test nào gửi token sai.
- Status: OPEN.

## ISS-014: Password rule at registration

- Type: AMBIGUOUS.
- Sources:
  - SRC-HURL `errors_auth.hurl`: for `PUT /user`, a comment cites the NIST 800-63B password policy ("min 8 chars, must accept at least 64 chars, no composition, no breach check"); passwords of 7 characters are rejected (422), 8 and 64 characters are accepted.
  - SRC-HURL: registration is only tested with blank passwords (422 `can't be blank`) and with `password123` (11 characters).
  - SRC-API: `NewUser.password` has no length constraint.
- Impact: It is not defined whether registration enforces the same minimum length.
- Options: (A) Same rule for registration and for update. (B) Rule only on update.
- Recommendation: A, as a proposed decision (PROP), because two different rules for one field would be inconsistent. The NIST reference exists only as a comment in a test file; the project treats it as a source of the Hurl-observable rule, not as a NASA or external requirement.
- Vietnamese: Quy tắc mật khẩu tối thiểu 8 ký tự chỉ được kiểm tra khi cập nhật, không kiểm tra khi đăng ký.
- Status: OPEN.

## ISS-015: Declared response statuses that no Hurl request exercises

- Type: UNVERIFIABLE.
- Source: `evidence/hurl-operation-coverage-2026-11-18.md` (generated by script from SRC-API and SRC-HURL).
- Facts: OpenAPI declares 70 response statuses across 19 operations. The Hurl suite asserts 53 of them (counts re-computed by script from the evidence table). The 17 not asserted are: `422` on 13 operations (GetCurrentUser, FollowUser, UnfollowUser, GetArticlesFeed, GetArticle, DeleteArticle, GetArticleComments, DeleteArticleComment, CreateArticleFavorite, DeleteArticleFavorite, GetTags, GetProfileByUsername, GetArticles); `401` on GetProfileByUsername, GetArticles, GetArticleComments; `409` on CreateArticle.
- Impact: The behavior for these statuses cannot be verified by the RealWorld suites. The requirements for them need a different verification method (project tests, inspection).
- Recommendation: Write requirements only for statuses with a trigger defined in a source. For the others, record as "declared, trigger unspecified" and decide per case (see ISS-012, ISS-013).
- Vietnamese: 17/70 mã trạng thái do OpenAPI khai báo không có test Hurl nào kiểm tra.
- Status: OPEN.

## ISS-016: Slug update on title change is only in prose

- Type: UNVERIFIABLE.
- Sources: SRC-DOC `endpoints.md` says the slug also gets updated when the title is changed. SRC-HURL `articles.hurl` updates only `body` (slug asserted unchanged) and never changes the title.
- Impact: The behavior has a source but no RealWorld test. SRC-E2E coverage: not yet read (see `phase1-status.md`).
- Recommendation: Keep the requirement (source: SRC-DOC), with verification by a project test.
- Vietnamese: Việc đổi slug khi đổi tiêu đề chỉ có trong tài liệu, không có test Hurl.
- Status: OPEN.

## ISS-017: Ordering, bounds, and defaults not specified

- Type: MISSING.
- Sources: SRC-DOC and SRC-API specify: article lists "ordered by most recent first"; `limit` default 20, minimum 1; `offset` minimum 0. SRC-HURL asserts ordering with two articles and `limit`/`offset` paging.
- Not specified in any source read: ordering of comments; ordering and size of the `GET /tags` result; maximum `limit`; behavior for `limit=0`, a negative `offset`, or a non-integer value; ordering of `tagList`  beyond what `articles.hurl` asserts (the order submitted is returned); allowed characters or length of tags.
- Impact: Design and tests need explicit values for these cases.
- Recommendation: Record each as a proposed decision (PROP) in the SRS, to be approved at QG1, and mark them as not derived from RealWorld.
- Vietnamese: Nhiều giới hạn và thứ tự chưa được nguồn nào quy định.
- Status: OPEN.

## ISS-018: Frontend routing prose versus the selector contract

- Type: CONFLICT.
- Sources: SRC-DOC `frontend/routing.md` lists `/`, `/login`, `/register`, `/settings`, `/editor`, `/editor/:slug`, `/article/:slug`, `/profile/:username`, `/profile/:username/favorites`. SRC-E2E `SELECTORS.md` Routes table also requires `/?feed=following`, `/?page=N`, `/tag/:tag`, and `/tag/:tag?page=N`.
- Impact: A frontend built only from `routing.md` fails the E2E suite.
- Recommendation: Follow SRC-E2E (higher precedence).
- Vietnamese: Tài liệu routing thiếu các route mà bộ E2E yêu cầu. Đề xuất theo `SELECTORS.md`.
- Status: OPEN.

## ISS-019: Text variants in labels

- Type: AMBIGUOUS (minor).
- Sources: `templates.md` shows "Favorite Post (29)" in the article banner and "Favorite Article (29)" in the article actions; `SELECTORS.md` lists `Favorite`, `Unfavorite`, and `Favorite Article`. `templates.md` shows the logout text "Or click here to logout." with a final period; `SELECTORS.md` lists "Or click here to logout" without it. `SELECTORS.md` says buttons are located by text with `:has-text()`.
- Impact: Low. Substring matching of the required text is expected to cover both forms; this is an inference and needs confirmation by running the suite.
- Recommendation: Use text that contains the `SELECTORS.md` strings. Verify when the E2E suite runs (Phase 4).
- Vietnamese: Có khác biệt nhỏ về chữ trên nút giữa `templates.md` và `SELECTORS.md`.
- Status: OPEN (minor).

## ISS-020: E2E error-handling tests mention HTTP 400, API spec uses 422

- Type: UNVERIFIABLE until read.
- Sources: SRC-E2E `error-handling.spec.ts` has test groups named "400 Bad Request" (login, registration, article creation). SRC-API and SRC-HURL define validation errors as 422. Only test titles have been read so far; the test bodies have not.
- Impact: Unknown. The frontend may be required to handle error responses it will never receive from this backend, or the test uses mocked responses.
- Action: Read `error-handling.spec.ts` before writing the frontend error-handling requirements (increment 2). Not a decision for the user yet.
- Vietnamese: Tên test nhắc đến lỗi 400, nhưng API trả 422. Tôi chưa đọc nội dung test nên chưa kết luận.
- Status: OPEN, waiting for the file to be read.
