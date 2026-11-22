# Routes

This page lists every route, the query parameters it accepts, and the fields of every form. The [application plan](application-plan.md) describes the pages, and the [database design](database-design.md) describes the data behind them.

## Routes

Route variables such as `<id>` are integers, written `<int:id>` in Flask.

| Method    | Route                   | Page or action                                         | Access            | File                 |
| :-------- | :---------------------- | :----------------------------------------------------- | :---------------- | :------------------- |
| GET       | `/`                     | Landing page                                           | Public            | `routes_requests.py` |
| GET, POST | `/register`             | Register                                               | Public            | `routes_auth.py`     |
| GET, POST | `/login`                | Login                                                  | Public            | `routes_auth.py`     |
| GET       | `/logout`               | Log out and return to Landing                          | Logged in         | `routes_auth.py`     |
| GET       | `/profile/<id>`         | View a profile                                         | Logged in         | `routes_profile.py`  |
| GET, POST | `/profile/edit`         | Edit my profile                                        | Logged in         | `routes_profile.py`  |
| GET       | `/posts`                | Browse and filter open team posts                      | Logged in         | `routes_posts.py`    |
| GET       | `/posts/<id>`           | Post detail, join form, and request list for the owner | Logged in         | `routes_posts.py`    |
| GET, POST | `/posts/new`            | Create a team post                                     | Logged in         | `routes_posts.py`    |
| GET, POST | `/posts/<id>/edit`      | Edit a team post                                       | Owner             | `routes_posts.py`    |
| POST      | `/posts/<id>/close`     | Close a team post                                      | Owner             | `routes_posts.py`    |
| POST      | `/posts/<id>/requests`  | Send a join request                                    | Logged in         | `routes_requests.py` |
| POST      | `/requests/<id>/accept` | Accept a join request                                  | Owner of the post | `routes_requests.py` |
| POST      | `/requests/<id>/reject` | Reject a join request                                  | Owner of the post | `routes_requests.py` |
| GET       | `/dashboard`            | My Team                                                | Logged in         | `routes_requests.py` |
| GET       | `/students` (optional)  | Find students by faculty and skill                     | Logged in         | `routes_profile.py`  |

Access levels:

- **Public:** anyone can open the route.
- **Logged in:** a visitor who is not logged in is sent to `/login`.
- **Owner:** the logged-in user must own the post. Anyone else receives a 403 response.

## Query Parameters

Query parameters are optional and are sent with GET. A parameter that is missing or empty is ignored, and several parameters combine with AND.

### `GET /posts`

| Parameter  | Type    | Example               | Description                                                          |
| :--------- | :------ | :-------------------- | :------------------------------------------------------------------- |
| `q`        | String  | `q=hackathon`         | Keyword searched in the competition name, organizer, and description |
| `category` | String  | `category=Technology` | Exact category from the fixed list                                   |
| `faculty`  | String  | `faculty=Fasilkom`    | Posts that want this faculty, or any faculty                         |
| `skill`    | Integer | `skill=3`             | Posts that look for the skill with this `id`                         |
| `upcoming` | Flag    | `upcoming=1`          | Only posts whose deadline has not passed                             |

The route lists only posts with `status` `open`. Example: `/posts?q=hackathon&faculty=Fasilkom&upcoming=1`.

### `GET /students` (optional)

| Parameter | Type    | Example      | Description                                |
| :-------- | :------ | :----------- | :----------------------------------------- |
| `faculty` | String  | `faculty=FK` | Students of this faculty                   |
| `skill`   | Integer | `skill=3`    | Students who have the skill with this `id` |

### Other routes

`/login` accepts a `next` parameter that holds the page to open after a successful login. It must be a path on this site, for example `next=/posts/5`.

## Form Fields

Every POST form includes a hidden `csrf_token` field, which Flask-WTF adds and checks. Fields marked with an asterisk are required.

### Register (`POST /register`)

| Field        | Type     | Validation                                         |
| :----------- | :------- | :------------------------------------------------- |
| `name`\*     | Text     | Not empty                                          |
| `email`\*    | Email    | Ends with `ui.ac.id` and is not already registered |
| `password`\* | Password | Not empty. Stored only as a hash                   |

### Login (`POST /login`)

| Field        | Type     | Validation                   |
| :----------- | :------- | :--------------------------- |
| `email`\*    | Email    | Matches a registered account |
| `password`\* | Password | Matches the stored hash      |

### Edit Profile (`POST /profile/edit`)

| Field                    | Type         | Validation                            |
| :----------------------- | :----------- | :------------------------------------ |
| `name`\*                 | Text         | Not empty                             |
| `faculty`\*              | Dropdown     | One value from the fixed faculty list |
| `study_program`\*        | Text         | Not empty                             |
| `class_year`\*           | Number       | A four-digit year                     |
| `bio`                    | Text area    | None                                  |
| `skills`                 | Multi-select | Zero or more existing skills          |
| `competition_experience` | Text area    | None                                  |
| `contact`\*              | Text         | Not empty                             |

### Create and Edit Post (`POST /posts/new`, `POST /posts/<id>/edit`)

Both routes use the same form. Edit Post fills it with the current values.

| Field                | Type         | Validation                                                                     |
| :------------------- | :----------- | :----------------------------------------------------------------------------- |
| `competition_name`\* | Text         | Not empty                                                                      |
| `organizer`          | Text         | None                                                                           |
| `category`\*         | Dropdown     | One value from the fixed category list                                         |
| `description`\*      | Text area    | Not empty                                                                      |
| `info_link`          | URL          | A valid URL when filled                                                        |
| `deadline`\*         | Date         | Sent as `YYYY-MM-DD` by the date picker                                        |
| `slots_needed`\*     | Number       | An integer of at least 1, and not less than the accepted requests when editing |
| `wanted_skills`      | Multi-select | Zero or more existing skills                                                   |
| `wanted_faculties`   | Multi-select | Zero or more values from the fixed faculty list                                |

### Join Request (`POST /posts/<id>/requests`)

| Field       | Type      | Validation |
| :---------- | :-------- | :--------- |
| `message`\* | Text area | Not empty  |

The route rejects the request when the post is closed, when the user owns the post, or when the user has already sent a request to it.

### Close, Accept, and Reject

`POST /posts/<id>/close`, `POST /requests/<id>/accept`, and `POST /requests/<id>/reject` are buttons with no fields other than `csrf_token`.

## Responses

| Case                                | Behavior                                                                          |
| :---------------------------------- | :-------------------------------------------------------------------------------- |
| Form is valid                       | Saves the data, flashes a success message, and redirects                          |
| Form is invalid                     | Shows the form again with the typed values and a message next to each wrong field |
| Not logged in                       | Redirects to `/login?next=<path>`                                                 |
| Not the owner                       | Returns 403                                                                       |
| Post, profile, or request not found | Returns 404                                                                       |

Redirect targets after a valid form:

| Route                                                         | Redirects to                        |
| :------------------------------------------------------------ | :---------------------------------- |
| `POST /register`                                              | `/login`                            |
| `POST /login`                                                 | `/dashboard`, or the `next` path    |
| `POST /profile/edit`                                          | `/profile/<id>` of the current user |
| `POST /posts/new`                                             | `/posts/<id>` of the new post       |
| `POST /posts/<id>/edit`                                       | `/posts/<id>`                       |
| `POST /posts/<id>/close`                                      | `/posts/<id>`                       |
| `POST /posts/<id>/requests`                                   | `/dashboard`                        |
| `POST /requests/<id>/accept` and `POST /requests/<id>/reject` | `/posts/<id>` of the request's post |

(An accepted request that fills the last slot also sets the post `status` to `closed`.)

