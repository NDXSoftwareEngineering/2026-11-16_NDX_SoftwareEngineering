# Frontend Source Analysis / Phân tích nguồn Frontend

Status / Trạng thái: **DRAFT** (increment 2, 2026-11-18). Author: Kiro.
This is an analysis of what the sources say. It is not the SRS and contains no numbered requirements.
Vietnamese summary / Tóm tắt: Tài liệu này ghi lại những gì bộ E2E, `SELECTORS.md`, `templates.md` và `routing.md` đòi hỏi ở frontend. Phát hiện quan trọng nhất là ở mục 2: bộ E2E gắn chặt với API công cộng của RealWorld, nên cách chạy bộ test này cho dự án cần được quyết định (ISS-021).

Sources (pinned submodule `realworld-main/`, commit `25588846`): SRC-E2E `specs/e2e/*.spec.ts`, `helpers/*.ts`, `SELECTORS.md`; SRC-DOC `frontend/templates.md`, `routing.md`, `styles.md`, `api.md`; SRC-THEME `assets/theme/styles.css` (not read).

## 1. What was read

| File | Read |
| --- | --- |
| `helpers/*.ts` (8 files) | in full |
| `articles`, `auth`, `comments`, `health`, `navigation`, `null-fields`, `settings`, `social`, `url-navigation`, `user-fetch-errors`, `xss-security` `.spec.ts` | in full |
| `error-handling.spec.ts` (911 lines) | lines 1 to 135 in full (the three "400" tests, the first "401" test, the helpers); the rest by test titles and by a search for every `page.route` call. Bodies of the other 27 tests were not read. |

## 2. How the E2E suite is meant to be run (FACT)

`helpers/config.ts` defines three test modes selected with the environment variable `TEST_MODE`:

| Mode | Meaning in the suite | `BROWSER_API` | `EXTERNAL_API` |
| --- | --- | --- | --- |
| `spa` (default) | The browser calls a REST API itself and holds the JWT; tests may intercept API traffic; cross-user scenarios use the demo backend's seeded users such as `johndoe` | true | true |
| `ssr` | A server renders pages and calls an external API; no browser API traffic | false | true |
| `fullstack` | The implementation owns frontend and backend; no assumption about a reachable API or seeded data; tests create their own users | false | false |

The default API base for test setup is `process.env.API_BASE || 'https://api.realworld.show/api'` (`helpers/config.ts`).

Three spec files hard-code the public demo API instead of using `API_BASE`:

| File | Hard-coded value | Tests affected |
| --- | --- | --- |
| `error-handling.spec.ts` line 8 | `const API_BASE = 'https://api.realworld.show/api'`; every `page.route` mock uses it | 31 |
| `user-fetch-errors.spec.ts` line 9 | `const API_BASE = 'https://api.realworld.show/api'` | 14 |
| `health.spec.ts` line 18 | `request.get('https://api.realworld.show/api/tags')` | 1 |

A mock registered for `https://api.realworld.show/api/...` only intercepts requests the frontend sends to that exact origin. A frontend that talks to a local backend (for example `http://localhost:8000/api`) is not intercepted, so those 45 mock-based tests cannot trigger the error they expect.

Tests skipped by mode (from `test.skip(...)` and `testInfo.skip(...)` statements, counted by search):

| Skip condition | Where | Static test declarations | Runtime tests (estimate) |
| --- | --- | --- | --- |
| `!BROWSER_API` | all of `error-handling.spec.ts`; all of `user-fetch-errors.spec.ts`; one test each in `articles`, `comments`, `auth` | 48 | 48 |
| `!EXTERNAL_API` | all of `xss-security.spec.ts` (5 declarations; the payload loops expand to 16 tests); one test in `health.spec.ts` | 6 | 17 |
| Total skipped in `fullstack` mode | | 54 of 133 | 65 of 144 (estimate) |
| Total skipped in `ssr` mode | | 48 of 133 | 48 of 144 (estimate) |

The runtime figure 144 is derived from the sources: 133 static declarations, where the XSS loops contribute 5 image payloads, 6 markdown payloads, 3 description payloads, and 2 single tests (16 in place of 5). The exact runtime count comes from `playwright test --list` in Phase 4.

Consequence for the project (ISS-021): the steering acceptance criterion "100% of RealWorld Playwright tests pass" does not state which mode, and in no mode can a frontend with a local backend satisfy all 133 tests unchanged.

## 3. Data the suite expects to exist before it runs (FACT)

| Expectation | Source test | Modes |
| --- | --- | --- |
| A user `johndoe` exists and has articles | `social.spec.ts`: follow and unfollow, view other user profile, followed users articles in feed | `spa`, `ssr` |
| The global feed contains articles from at least 2 different authors, each with a loading avatar | `null-fields.spec.ts` "author avatars should render on other user articles in feed" | all |
| The global feed contains at least one article by another user (to favorite, and to check comment delete rights) | `articles.spec.ts` favorite and unfavorite; `comments.spec.ts` "only allow comment author to delete"; `social.spec.ts` "favorited articles on profile" | all |
| The global feed shows articles without the test creating any (`/` shows Global Feed with article previews within 2 s) | `url-navigation.spec.ts` | all |
| The sidebar has at least one tag | `url-navigation.spec.ts` "/tag/:tag should filter by tag" | all |

In `fullstack` mode some of these can be met by articles left by earlier tests, which makes them order-dependent. This is a test environment need, not a product requirement (ISS-022).

## 4. Page behavior observed in the tests (FACT)

References are `file: test title`.

### 4.1 Authentication and session

| Observed behavior | Source |
| --- | --- |
| After successful registration or login the browser URL is `/`; the navbar shows a link `a[href="/profile/<username>"]` | `auth`: register, login |
| Registration with a malformed email leaves the user on `/register`, not logged in | `auth`: malformed email |
| Failed login keeps the user on `/login` and shows `.error-messages` | `auth`: invalid login, wrong password |
| Logout is done from `/settings` with a button containing "Or click here to logout", then the URL is `/` and the navbar shows Sign in and Sign up | `auth`, `helpers/auth.ts` |
| `/editor` when not logged in redirects away from `/editor` | `auth`: prevent accessing editor |
| `/?feed=following` when not logged in redirects to `/login` | `url-navigation` |
| The session survives a page reload | `auth`: maintain session |
| The JWT is stored in `localStorage` under the key `jwtToken` | `SELECTORS.md`; `auth`, `user-fetch-errors` |
| An invalid token in `localStorage` is cleared on reload; `getToken()` returns null and `getAuthState()` is `unauthenticated`; the app shows Sign in and Sign up | `auth`: invalid token on reload |
| `window.__conduit_debug__` exposes `getToken()`, `getAuthState()` (`authenticated`, `unauthenticated`, `unavailable`, `loading`), and `getCurrentUser()` | `SELECTORS.md`, `helpers/debug.ts` |

### 4.2 Startup behavior when `GET /user` fails (all use mocks on the demo URL, see §2)

| Failure of `GET /api/user` with a stored token | Expected frontend behavior | Source |
| --- | --- | --- |
| 400, 401, 403, 404 | Logged-out state: the navbar shows Sign in and Sign up; the token is cleared; for 401 the article list still loads and `getAuthState()` is `unauthenticated` | `user-fetch-errors` |
| 500, network timeout, connection refused, malformed JSON | "Unavailable" mode: the page shows the text `Connecting`; the token is kept (also after reload); the app stays usable (Global Feed visible, tags clickable, Home link works) | `user-fetch-errors` |
| Empty 200 body | The navbar is still visible (no crash) | `user-fetch-errors` |
| 401 on `/settings` or `/editor` | The browser leaves that URL and the navbar is visible | `user-fetch-errors` |
| 500 or network error on `/settings` | The navbar is still visible | `user-fetch-errors` |

### 4.3 Error display (mock-based)

| Observed behavior | Source |
| --- | --- |
| A 400 response with `{errors: {...}}` on login, register, or article creation shows `.error-messages`, keeps the user on the same page, and the form fields remain | `error-handling`: three "400" tests (lines 36 to 100) |
| A 401 on `PUT /user` shows `.error-messages` and keeps the settings form usable | `error-handling` line 104 |
| Titles of the other tests: 401 on posting a comment; 403 on updating an article, deleting another user's comment, following a user; 500 on feed, tags, profile, article detail, settings; network errors on the same screens, on forms, favorite, and follow; timeout and connection refused; malformed JSON; empty body; 404 for a non-existent article and profile | `error-handling` (titles only; bodies not read) |

ISS-020 is answered by this reading: the "400" tests use mocked responses and do not conflict with the 422 returned by the backend. The frontend has to display the `errors` body of any failed response (status 400, 401, 403, 404, 422, 5xx are all exercised in mocks) in `.error-messages` without crashing.

### 4.4 Home, feeds, tags, pagination

| Observed behavior | Source |
| --- | --- |
| `/` shows the Global Feed tab with class `active` and at least one article preview | `url-navigation` |
| The Your Feed tab links to `/?feed=following`; the Global Feed tab links to `/`; clicking switches the URL and the active class | `url-navigation` |
| An empty Your Feed shows `.empty-feed-message` that contains the text "Your feed is empty" and a link `a[href="/"]` | `url-navigation` |
| `/tag/:tag` shows a tab (`.nav-link`) with the tag text, active, and filters the articles; clicking a tag in `.sidebar .tag-list .tag-pill` does the same | `url-navigation`, `navigation` |
| Tags of a newly created article appear in the sidebar tag list | `navigation`: display popular tags |
| The article list shows **10 articles per page**; with 15 articles the pagination has a page "2" link (`.pagination .page-link`), page 1 shows 10 and page 2 shows 5 | `url-navigation`, `navigation` |
| The pagination URL is `?page=N` (and `/?feed=following&page=2`, `/tag/<tag>?page=2`); the current page's `.page-item` has class `active`; opening `?page=2` directly loads page 2; switching to Global Feed resets the page | `url-navigation` |
| An article preview contains `h1` (title), `p` (description), `.author`, tags in `.tag-list .tag-default`, and a favorite button; clicking the author name opens `/profile/<username>` | `articles`, `navigation` |

### 4.5 Articles, editor, favorites, comments

| Observed behavior | Source |
| --- | --- |
| Editor form fields: `input[name="title"]`, `input[name="description"]`, `textarea[name="body"]`, tag input `input[placeholder="Enter tags"]` (a tag is added with the Enter key); the submit button text is "Publish Article"; after publishing the URL is `/article/<slug>` | `helpers/articles.ts`, `SELECTORS.md` |
| Editing at `/editor/<slug>` pre-fills the form from the API; tags can be removed by clicking the `i` icon in a tag pill; if all tags are removed the `PUT` body contains `tagList: []` (not omitted) | `articles`: remove all tags |
| The article page shows `.article-page`, `h1` with the title, `.article-content p` with the body, and tags | `articles` |
| The author sees "Edit Article" and "Delete Article"; other users and anonymous users do not | `articles`: only allow author |
| Deleting an article navigates to `/`; the frontend must treat HTTP 200 and 204 alike (any 2xx) | `articles` |
| Favorite button `button.btn-outline-primary` with text "Favorite" becomes `button.btn-primary` with text "Unfavorite" and back; the favorite button also works inside an article preview in the feed | `articles`, `navigation`, `SELECTORS.md` |
| The comment form `textarea[placeholder="Write a comment..."]` and button "Post Comment" exist only for logged-in users; anonymous users see a link to `/login` instead of the form | `comments` |
| Posted comments appear in `.card:not(.comment-form) .card-block`; the form is cleared after posting; long comments (about 1,450 characters) display; comments persist after reload | `comments` |
| The delete icon (`span.mod-options i.ion-trash-a`) shows only on the user's own comments; deleting accepts HTTP 200 or 204 | `comments` |

### 4.6 Profile and settings

| Observed behavior | Source |
| --- | --- |
| A profile page shows `h4` with the username, `.user-info`, `.user-img`; own profile shows a link `a[href="/settings"]` with "Edit Profile Settings" and no Follow button; another user's profile shows a Follow button | `social` |
| Follow turns the button into Unfollow and back; the button text is "Follow <username>" or "Unfollow <username>" | `social`, `SELECTORS.md` |
| Profile tabs: "My Articles" (articles by the author) and "Favorited" linking to `/profile/<username>/favorites` | `social`, `navigation`, `routing.md` |
| Settings form fields: `input[name="image"]`, `input[name="username"]`, `textarea[name="bio"]`, `input[name="email"]`, `input[name="password"]`; submit `button[type="submit"]` with text "Update Settings"; the form is pre-filled; null image and bio show as an empty string | `settings`, `null-fields`, `helpers/profile.ts` |
| After saving settings the URL becomes `/profile/<username>`, the navbar still shows the username, and `getCurrentUser()` has the new values | `settings` |
| A user without an image shows an avatar whose `src` contains `default-avatar.svg` in these places: `.user-img` (profile), `nav .user-pic`, `.article-meta img`, `.comment-author-img` (comment form and posted comments); clearing the image to an empty string restores the default avatar | `null-fields`, `SELECTORS.md` |
| A null bio shows as empty text, never the literal "null" | `null-fields` |

### 4.7 Security

| Observed behavior | Source |
| --- | --- |
| For each of 5 image-URL payloads (including `"onerror=`, `javascript:`, `data:text/html,<script>`), the profile page, the navbar image and the comment form image have no `onerror` or `onload` attribute and no dialog opens | `xss-security` |
| For HTML in the article description (3 payloads) and in the article body (6 payloads: script, img onerror, svg onload, iframe srcdoc, javascript: href, onmouseover), no dialog opens, the article preview or content contains no `script` element, and the article content contains no element with `onerror`, `onload`, `onmouseover`, `onclick`, or `onfocus` | `xss-security` |
| The payloads are written through the API and stored by the backend, then rendered by the frontend. The test helper throws if the API answers with a non-2xx status, so the backend must accept these values (ISS-023) | `xss-security`, `helpers/api.ts` |

## 5. Selectors and strings required by tests but absent from `SELECTORS.md` (FACT, ISS-024)

| Required by the tests | Test |
| --- | --- |
| Text `Connecting` visible in unavailable mode | `user-fetch-errors` |
| Text "Your feed is empty" inside `.empty-feed-message` | `url-navigation` |
| Submit buttons located with `button[type="submit"]` on register, login, and settings | `helpers/auth.ts`, `helpers/profile.ts` |
| Class `active` on the current tab link and page item | `url-navigation` (listed for `.page-item` in `SELECTORS.md`, not for `.nav-link`) |
| `h4` for the profile username, `.article-page h1`, `.article-content p`, `a.navbar-brand`, `nav.navbar`, `nav .user-pic` | `social`, `articles`, `health`, `null-fields` |
| Pagination links `.pagination .page-link` | `url-navigation` (`SELECTORS.md` lists `.pagination` and `.page-item` only) |

## 6. Statements in `routing.md` and `templates.md` (FACT)

- `routing.md` states that article Markdown is rendered client-side, that the delete article button shows only to the author, and that the delete comment button shows only to the comment's author.
- `templates.md` gives the HTML structure for header (logged out and logged in), footer, Home, Login, Register, Profile, Settings, Editor, and Article pages. It loads the shared theme, fonts (Source Sans Pro, Lora), and the legacy Ionicons 2.0.1 icons. The article page contains a TODO comment ("update to switch between follow/favorite AND edit/delete").
- `templates.md` does not specify the page size, the `Connecting` text, or the empty-feed message.
- `styles.md` requires the shared theme `styles.css`; fonts and icons are not bundled in it; the default avatar is `assets/media/default-avatar.svg`.

## 7. Open points raised

See `issues-log.md`: ISS-018 (routes), ISS-019 (labels), ISS-020 (answered), ISS-021 (mode and hard-coded URL), ISS-022 (seed data), ISS-023 (verbatim storage), ISS-024 (selector contract incomplete).
