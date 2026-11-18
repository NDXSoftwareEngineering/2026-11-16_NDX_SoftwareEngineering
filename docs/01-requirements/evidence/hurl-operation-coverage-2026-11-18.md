RealWorld API operation coverage by the Hurl suite
Generated 2026-11-18 from realworld-main/specs/api (openapi.yml + hurl/*.hurl). Pinned commit 25588846.
Method: OpenAPI operations parsed from openapi.yml; Hurl requests normalised by path pattern; status = first "HTTP nnn" line after the request.
Hurl requests found: 157; OpenAPI operations: 19; unmapped Hurl requests: 0

| # | Operation | Method path | Auth (OpenAPI security) | OpenAPI responses | Hurl calls | Hurl statuses asserted | Hurl files |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | Login | POST /users/login | not required / optional | 200, 401, 422 | 5 | 200 x1, 401 x1, 422 x3 | auth.hurl, errors_auth.hurl |
| 2 | CreateUser | POST /users | not required / optional | 201, 409, 422 | 22 | 201 x16, 409 x2, 422 x4 | articles.hurl, auth.hurl, comments.hurl, errors_articles.hurl, errors_auth.hurl, errors_authorization.hurl, errors_comments.hurl, errors_profiles.hurl, favorites.hurl, feed.hurl, pagination.hurl, profiles.hurl, tags.hurl |
| 3 | GetCurrentUser | GET /user | required | 200, 401, 422 | 9 | 200 x8, 401 x1 | auth.hurl, errors_auth.hurl |
| 4 | UpdateCurrentUser | PUT /user | required | 200, 401, 422 | 21 | 200 x12, 401 x1, 422 x8 | auth.hurl, errors_auth.hurl |
| 5 | GetProfileByUsername | GET /profiles/{username} | not required / optional | 200, 401, 404, 422 | 4 | 200 x3, 404 x1 | errors_profiles.hurl, profiles.hurl |
| 6 | FollowUserByUsername | POST /profiles/{username}/follow | required | 200, 401, 404, 422 | 4 | 200 x2, 401 x1, 404 x1 | errors_profiles.hurl, feed.hurl, profiles.hurl |
| 7 | UnfollowUserByUsername | DELETE /profiles/{username}/follow | required | 200, 401, 404, 422 | 4 | 200 x2, 401 x1, 404 x1 | errors_profiles.hurl, feed.hurl, profiles.hurl |
| 8 | GetArticlesFeed | GET /articles/feed | required | 200, 401, 422 | 5 | 200 x4, 401 x1 | errors_articles.hurl, feed.hurl |
| 9 | GetArticles | GET /articles | not required / optional | 200, 401, 422 | 10 | 200 x10 | articles.hurl, favorites.hurl, pagination.hurl |
| 10 | CreateArticle | POST /articles | required | 201, 401, 409, 422 | 16 | 201 x12, 401 x1, 422 x3 | articles.hurl, comments.hurl, errors_articles.hurl, errors_authorization.hurl, errors_comments.hurl, favorites.hurl, feed.hurl, pagination.hurl, tags.hurl |
| 11 | GetArticle | GET /articles/{slug} | not required / optional | 200, 404, 422 | 7 | 200 x5, 404 x2 | articles.hurl, errors_articles.hurl, favorites.hurl |
| 12 | UpdateArticle | PUT /articles/{slug} | required | 200, 401, 403, 404, 422 | 8 | 200 x3, 401 x1, 403 x1, 404 x2, 422 x1 | articles.hurl, errors_articles.hurl, errors_authorization.hurl |
| 13 | DeleteArticle | DELETE /articles/{slug} | required | 204, 401, 403, 404, 422 | 15 | 204 x12, 401 x1, 403 x1, 404 x1 | articles.hurl, comments.hurl, errors_articles.hurl, errors_authorization.hurl, errors_comments.hurl, favorites.hurl, feed.hurl, pagination.hurl, tags.hurl |
| 14 | GetArticleComments | GET /articles/{slug}/comments | not required / optional | 200, 401, 404, 422 | 7 | 200 x6, 404 x1 | comments.hurl, errors_authorization.hurl, errors_comments.hurl |
| 15 | CreateArticleComment | POST /articles/{slug}/comments | required | 201, 401, 404, 422 | 7 | 201 x4, 401 x1, 404 x1, 422 x1 | comments.hurl, errors_authorization.hurl, errors_comments.hurl |
| 16 | DeleteArticleComment | DELETE /articles/{slug}/comments/{id} | required | 204, 401, 403, 404, 422 | 6 | 204 x2, 401 x1, 403 x1, 404 x2 | comments.hurl, errors_authorization.hurl, errors_comments.hurl |
| 17 | CreateArticleFavorite | POST /articles/{slug}/favorite | required | 200, 401, 404, 422 | 3 | 200 x1, 401 x1, 404 x1 | errors_articles.hurl, favorites.hurl |
| 18 | DeleteArticleFavorite | DELETE /articles/{slug}/favorite | required | 200, 401, 404, 422 | 3 | 200 x1, 401 x1, 404 x1 | errors_articles.hurl, favorites.hurl |
| 19 | GetTags | GET /tags | not required / optional | 200, 422 | 1 | 200 x1 | tags.hurl |

Operations with at least one Hurl call: 19 / 19
Unmapped Hurl requests: none
