# TemuTim

TemuTim is a competition teammate finder web application for Universitas Indonesia students. Its goal is to connect students who need teammates for a competition with students from any faculty who have the skills they are looking for.

> A project for Dasar-Dasar Pemrograman 1 (DDP1), Universitas Indonesia. Kelompok 1 | RAI | DDP-A.

## Requirements Coverage

| Requirement               | Status   | Behavior                                                                                                                                                                        |
| :------------------------ | :------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Registration and login    | Required | Registration accepts only emails ending in `ui.ac.id`. Passwords are stored as hashes. Every page except landing, login, and register requires login.                           |
| Student profile           | Required | Stores name, faculty, study program, class year, bio, skills, competition experience, and contact. Any logged-in student can view a profile, and only its owner can edit it.    |
| Team post creation        | Required | A student posts a competition they want a team for: name, organizer, category, description, info link, deadline, number of members needed, and the skills and faculties wanted. |
| Post editing and closing  | Required | Only the owner of a post can edit it or close it.                                                                                                                               |
| Browse and filter         | Required | `GET /posts` lists open posts with keyword search and filters for category, faculty, skill, and deadline not yet passed. `GET /posts/<id>` shows one post.                      |
| Join request              | Required | A student sends a request with a short message. The post owner accepts or rejects it, and the remaining slots update automatically.                                             |
| My Team                   | Required | `GET /dashboard` shows posts I created, posts I applied to or joined, request statuses (pending, accepted, rejected), and team contacts.                                        |
| Contact privacy           | Required | Contact details are hidden on profiles and post pages. They appear only to members of the same team after a join request is accepted.                                           |
| Find students             | Optional | `GET /students` lists profiles filtered by faculty and skill.                                                                                                                   |
| Skill match score         | Optional | Compares a student's skills with a post's wanted skills using set intersection.                                                                                                 |
| Auto-close after deadline | Optional | Posts past their deadline switch to closed.                                                                                                                                     |

## Documentation

| Document                                                 | Contents                                                           | Status  |
| :------------------------------------------------------- | :----------------------------------------------------------------- | :------ |
| [`docs/project-plan.md`](docs/project-plan.md)           | Team responsibilities, work split, and checkpoint schedule         | Planned |
| [`docs/application-plan.md`](docs/application-plan.md)   | Pages, shared layout, and form behavior                            | Planned |
| [`docs/database-design.md`](docs/database-design.md)     | Entities, relationships, and business rules                        | Planned |
| [`docs/routes.md`](docs/routes.md)                       | Routes, query parameters, and form fields                          | Planned |
| [`docs/development-setup.md`](docs/development-setup.md) | Prerequisites and environment variables                            | Planned |
| [`docs/contributors.md`](docs/contributors.md)           | GitHub commit attribution and team accounts                        | Planned |

## Repository Layout

| Path                  | Contents                                             |
| :-------------------- | :--------------------------------------------------- |
| `app.py`              | Creates the Flask app and registers the blueprints   |
| `extensions.py`       | Shared `db` and `login_manager` objects              |
| `models.py`           | `User`, `Skill`, `TeamPost`, `JoinRequest`           |
| `routes_auth.py`      | Register, login, logout                              |
| `routes_profile.py`   | View and edit profile                                |
| `routes_posts.py`     | Create, edit, close, browse, and filter team posts   |
| `routes_requests.py`  | Join requests and the My Team dashboard              |
| `templates/`          | `base.html` plus one folder per feature              |
| `static/`             | CSS and images                                       |
| `seed.py`             | Sample posts and profiles for demos                  |
| `docs/`               | Project documentation                                |

## Tech Stack

| Component       | Technology                                 |
| :-------------- | :----------------------------------------- |
| Language        | Python 3                                   |
| Web framework   | Flask with Blueprints                      |
| Templates       | HTML, CSS, Jinja2, Bootstrap 5 (CDN)       |
| Database        | SQLite                                     |
| ORM             | Flask-SQLAlchemy                           |
| Authentication  | Flask-Login, Werkzeug password hashing     |
| Forms           | Flask-WTF (validation and CSRF protection) |
| Design          | Figma (wireframes)                         |
| Hosting         | PythonAnywhere (free account)              |
| Version control | Git and GitHub                             |

## Pages

| Route                  | Page                                                     | Access    |
| :--------------------- | :------------------------------------------------------- | :-------- |
| `/`                    | Landing page                                             | Public    |
| `/register`, `/login`  | Register and login                                       | Public    |
| `/logout`              | Logout                                                   | Logged in |
| `/dashboard`           | My Team                                                  | Logged in |
| `/posts`               | Browse and filter team posts                             | Logged in |
| `/posts/<id>`          | Post detail, join button, and request list for the owner | Logged in |
| `/posts/new`           | Create a team post                                       | Logged in |
| `/posts/<id>/edit`     | Edit or close a team post                                | Owner     |
| `/profile/<id>`        | View a profile                                           | Logged in |
| `/profile/edit`        | Edit my profile                                          | Logged in |
| `/students` (optional) | Find students by faculty and skill                       | Logged in |

## Data Model

| Entity       | Key fields                                                                                                                                         |
| :----------- | :------------------------------------------------------------------------------------------------------------------------------------------------- |
| `User`       | `id`, `name`, `email` (unique), `password_hash`, `faculty`, `study_program`, `class_year`, `bio`, `contact`, `competition_experience`              |
| `Skill`      | `id`, `name` (unique)                                                                                                                              |
| `user_skill` | `user_id`, `skill_id` (many-to-many between `User` and `Skill`)                                                                                    |
| `TeamPost`   | `id`, `owner_id`, `competition_name`, `organizer`, `category`, `description`, `info_link`, `deadline`, `slots_needed`, `wanted_faculties`, `status` (`open` or `closed`), `created_at` |
| `post_skill` | `post_id`, `skill_id` (skills wanted by a post)                                                                                                    |
| `JoinRequest`| `id`, `post_id`, `applicant_id`, `message`, `status` (`pending`, `accepted`, or `rejected`), `created_at`                                          |

### Core Rules

| Rule             | Detail                                                                                          |
| :--------------- | :---------------------------------------------------------------------------------------------- |
| Team membership  | A team is the post owner plus every applicant whose request is `accepted`.                      |
| Slots            | Remaining slots equal `slots_needed` minus accepted requests. At zero, the post closes itself.  |
| Request limits   | One request per student per post. An owner cannot apply to their own post.                      |
| Permissions      | Only the post owner can edit or close a post and accept or reject its requests.                 |
| Contact privacy  | Contacts are shown only between members of the same team after acceptance.                      |

## Team

| Member                    | GitHub                                                              | Track                                          |
| :------------------------ | :------------------------------------------------------------------ | :--------------------------------------------- |
| Salva Faddil Rinaldi      | [salvafaddil](https://github.com/salvafaddil)                       | Foundation, authentication, and profile        |
| Iman Riza Wiramdhan       | [rizawiramdhan25-glitch](https://github.com/rizawiramdhan25-glitch) | Team posts, browse and filter, and sample data |
| Keyla Batti Putri Anggoro | TBD                                                                 | Join requests, My Team, and landing page       |

## Checkpoint Status

| Checkpoint                                           | Deadline         | Status      |
| :--------------------------------------------------- | :--------------- | :---------- |
| Project draft                                        | 22 November 2026 | Complete    |
| 0. Foundation: wireframes, models, deployed skeleton | 23 November 2026 | Not started |
| 1. Parallel feature work                             | 24 December 2026 | Not started |
| 2. Integration and cross-testing                     | 25 December 2026 | Not started |
| 3. Final deployment, report, and presentation prep   | 26 December 2026 | Not started |
| Final submission                                     | 10 December 2026 | Not started |

## Local Development

| Item                          | Detail                                                                |
| :---------------------------- | :-------------------------------------------------------------------- |
| Prerequisites and environment | [`docs/development-setup.md`](docs/development-setup.md)              |
| Local URL                     | `http://127.0.0.1:5000`                                               |
| Routes and form fields        | [`docs/routes.md`](docs/routes.md)                                    |
| Data storage                  | SQLite file at `instance/temutim.db`, created automatically on first run |

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
python seed.py                  # optional: load sample data
flask --app app run --debug
```

## Public Deployment

| Item       | Value                                                                                         |
| :--------- | :-------------------------------------------------------------------------------------------- |
| Host       | PythonAnywhere free account                                                                   |
| Site       | `https://<username>.pythonanywhere.com` (to be set)                                           |
| Update     | Run `git pull` in a PythonAnywhere Bash console, then press Reload on the Web tab             |
| Database   | SQLite file on PythonAnywhere's persistent file system                                        |

Free account limits to keep in mind (check them again at deploy time, since plans can change):

| Limit              | Effect on the project                                                    |
| :----------------- | :----------------------------------------------------------------------- |
| One web app        | The whole site runs as a single Flask app.                               |
| 1-month expiry     | The web app must be extended manually every month.                       |
| Restricted outbound internet | The app must not depend on external APIs.                      |
| No free MySQL for new accounts | SQLite is used instead.                                      |
