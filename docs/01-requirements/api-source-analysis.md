# API Source Analysis / Phân tích nguồn API

Status / Trạng thái: **DRAFT** (increment 1, 2026-11-18). Author: Kiro.
This is an analysis of what the sources say. It is not the SRS and contains no numbered requirements. Requirements are written in the next increment and traced to this analysis.
Vietnamese summary / Tóm tắt: Tài liệu này liệt kê những gì các nguồn RealWorld quy định về API (19 thao tác), đối chiếu giữa OpenAPI, Hurl và tài liệu. Chưa có yêu cầu đánh số nào ở đây.

Sources (all in the pinned submodule `realworld-main/`, commit `25588846`): SRC-API `specs/api/openapi.yml`; SRC-HURL `specs/api/hurl/*.hurl`; SRC-DOC `docs/src/content/docs/specifications/backend/*.md`.

## 1. Operation catalog (FACT)

Columns come from the generated table in `evidence/hurl-operation-coverage-2026-11-18.md`. "Auth" is taken from OpenAPI `security` plus the description text.

| # | Operation | Method and path | Auth | Success | Error statuses declared in OpenAPI |
| --- | --- | --- | --- | --- | --- |
| 1 | Login | POST /users/login | none | 200 | 401, 422 |
| 2 | CreateUser | POST /users | none | 201 | 409, 422 |
| 3 | GetCurrentUser | GET /user | required | 200 | 401, 422 |
| 4 | UpdateCurrentUser | PUT /user | required | 200 | 401, 422 |
| 5 | GetProfileByUsername | GET /profiles/{username} | optional | 200 | 401, 404, 422 |
| 6 | FollowUserByUsername | POST /profiles/{username}/follow | required | 200 | 401, 404, 422 |
| 7 | UnfollowUserByUsername | DELETE /profiles/{username}/follow | required | 200 | 401, 404, 422 |
| 8 | GetArticlesFeed | GET /articles/feed | required | 200 | 401, 422 |
| 9 | GetArticles | GET /articles | optional | 200 | 401, 422 |
| 10 | CreateArticle | POST /articles | required | 201 | 401, 409, 422 |
| 11 | GetArticle | GET /articles/{slug} | none | 200 | 404, 422 |
| 12 | UpdateArticle | PUT /articles/{slug} | required | 200 | 401, 403, 404, 422 |
| 13 | DeleteArticle | DELETE /articles/{slug} | required | 204 | 401, 403, 404, 422 |
| 14 | GetArticleComments | GET /articles/{slug}/comments | optional | 200 | 401, 404, 422 |
| 15 | CreateArticleComment | POST /articles/{slug}/comments | required | 201 | 401, 404, 422 |
| 16 | DeleteArticleComment | DELETE /articles/{slug}/comments/{id} | required | 204 | 401, 403, 404, 422 |
| 17 | CreateArticleFavorite | POST /articles/{slug}/favorite | required | 200 | 401, 404, 422 |
| 18 | DeleteArticleFavorite | DELETE /articles/{slug}/favorite | required | 200 | 401, 404, 422 |
| 19 | GetTags | GET /tags | none | 200 | 422 |

Measured coverage by Hurl (script output, 2026-11-18): 157 Hurl requests in 13 files; all 157 map to one of the 19 operations; all 19 operations have at least one Hurl request; 53 of the 70 declared statuses are asserted (ISS-015).

## 2. Data shapes (FACT, from OpenAPI schemas and `api-response-format.md`)

| Object | Fields (required unless stated) |
| --- | --- |
| User | `email`, `token`, `username`, `bio` (string or null), `image` (string or null) |
| Profile | `username`, `bio` (nullable), `image` (nullable), `following` (boolean) |
| Article | `slug`, `title`, `description`, `body`, `tagList` (array of strings), `createdAt`, `updatedAt` (date-time), `favorited` (boolean), `favoritesCount` (integer), `author` (Profile) |
| Article in a list | Same as Article without `body` (the docs state the body was removed from list endpoints on 2024-08-16; Hurl asserts `$.articles[0].body` does not exist) |
| Comment | `id` (integer), `createdAt`, `updatedAt`, `body`, `author` (Profile) |
| Lists | `{articles, articlesCount}`, `{comments}`, `{tags}` |
| Error | `{errors: {<field or resource>: [message, ...]}}` |

Request bodies: `{user: {...}}`, `{article: {...}}`, `{comment: {body}}`. Required: login `email`, `password`; register `username`, `email`, `password`; create article `title`, `description`, `body` (optional `tagList`); comment `body`. PUT user: "at least one field is required" (OpenAPI), accepted fields `email`, `username`, `password`, `image`, `bio` (docs).

Authentication: header `Authorization: Token <jwt>`. A token is returned by registration and login. Content type should be `application/json; charset=utf-8` (docs).

## 3. Behavior asserted by Hurl and not visible in OpenAPI (FACT, Hurl is authoritative)

| Behavior | Hurl source |
| --- | --- |
| Updating `bio` or `image` to an empty string stores null; setting them to null is accepted | `auth.hurl` |
| Updating `email`, `username`, or `password` to an empty string or null is rejected with 422; a malformed email gives 422 `is invalid` | `errors_auth.hurl` |
| Password at update: 7 characters rejected; 8 and 64 characters accepted | `errors_auth.hurl` (ISS-014) |
| Duplicate username or email at registration gives 409 `has already been taken` per field | `errors_auth.hurl` (ISS-011) |
| Wrong password gives 401 `errors.credentials: ["invalid"]` | `errors_auth.hurl` |
| Missing token gives 401 `errors.token: ["is missing"]` on every protected endpoint tested | `errors_*.hurl` |
| Blank required field gives 422 `errors.<field>: ["can't be blank"]` (username, email, password, title, description, body, comment body) | `errors_auth.hurl`, `errors_articles.hurl`, `errors_comments.hurl` |
| Unknown article, comment, profile gives 404 `errors.article`, `errors.comment`, `errors.profile: ["not found"]` | `errors_*.hurl` |
| Non-author update or delete of an article gives 403 `errors.article: ["forbidden"]`; non-author delete of a comment gives 403 `errors.comment: ["forbidden"]` and the comment survives | `errors_authorization.hurl` |
| Duplicate article titles are allowed and get distinct slugs | `errors_articles.hurl` (ISS-012) |
| Update without `tagList` keeps tags; `tagList: []` removes all tags; `tagList: null` gives 422 | `articles.hurl` |
| `createdAt` is unchanged and `updatedAt` changes after an article update | `articles.hurl` |
| Lists are ordered most recent first; `limit` and `offset` work on `/articles` and `/articles/feed`; `articlesCount` is the total, not the page size | `pagination.hurl`, `feed.hurl` |
| Feed for a new user is empty; feed contains only articles of followed users; `author.following` is true for them | `feed.hurl` |
| Favorite and unfavorite are reflected in `favorited` and `favoritesCount` and persist; `?favorited=<username>` filters | `favorites.hurl` |
| Follow and unfollow return the profile with `following` true or false and persist; profile reads work with and without a token | `profiles.hurl` |
| `GET /tags` returns a list of strings that contains the tags of a newly created article (the test creates two tags and finds both) | `tags.hurl` |
| Comments: create returns 201 with id; list works with and without a token; delete returns 204; deleting one of two comments leaves the other | `comments.hurl` |
| Deleting an article returns 204; reading it afterwards gives 404 | `articles.hurl` |

## 4. Behavior stated only in prose (SRC-DOC) with no Hurl assertion found

| Behavior | Source | Note |
| --- | --- | --- |
| Slug changes when the title changes | `endpoints.md` | ISS-016 |
| CORS: if frontend and backend run on different host or port, handle `OPTIONS` and return `Access-Control-Allow-Origin` and `Access-Control-Allow-Headers` | `cors.md` | No Hurl or OpenAPI coverage; applies to this project because the planned frontend and backend are separate servers (design decision, Phase 2) |
| `Content-Type: application/json; charset=utf-8` on responses | `api-response-format.md` | Not asserted in the Hurl files read |
| Article `body` is rendered as Markdown on the client | `routing.md` | Frontend |

## 5. Open points raised by this analysis

See `issues-log.md`: ISS-011 to ISS-018 relate to this analysis.
