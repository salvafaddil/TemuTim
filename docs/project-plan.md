# Project Plan

This page records the team structure, work split, and checkpoint schedule for TemuTim. The [database design](database-design.md) and [routes](routes.md) pages describe the technical design.

## Team and Responsibilities

| Member                    | Track                                          | Responsibilities                                                                                                                                                                                |
| :------------------------ | :--------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Salva Faddil Rinaldi      | Foundation, authentication, and profile        | Sets up the repository, project structure, `models.py`, and `base.html`, deploys the empty skeleton, builds registration, login, and the student profile, and merges Pull Requests into `main`. |
| Iman Riza Wiramdhan       | Team posts, browse and filter, and sample data | Builds team post creation, editing, and closing, the browse page with search and filters, the post detail page, and the demo data in `seed.py`.                                                 |
| Keyla Batti Putri Anggoro | Join requests, My Team, and landing page       | Builds the join request flow, the owner's accept and reject controls, the My Team dashboard, and the landing page.                                                                              |

## Work Split

Each member builds their features end to end (page, route, and database query) instead of splitting frontend and backend. This keeps members from waiting on each other and keeps the final merge small.

| Area              | Owner                     | Scope                                                                                                     |
| :---------------- | :------------------------ | :-------------------------------------------------------------------------------------------------------- |
| Foundation        | Salva Faddil Rinaldi      | `app.py`, `extensions.py`, `models.py`, `base.html`, and the first deployment                             |
| Authentication    | Salva Faddil Rinaldi      | `routes_auth.py`: `ui.ac.id` email check, password hashing, login, logout, and login-required pages       |
| Profile           | Salva Faddil Rinaldi      | `routes_profile.py`: `/profile/<id>` and `/profile/edit`, with skills and competition experience          |
| Team posts        | Iman Riza Wiramdhan       | `routes_posts.py`: `/posts/new` and `/posts/<id>/edit`, owner-only edit and close                         |
| Browse and filter | Iman Riza Wiramdhan       | `routes_posts.py`: `GET /posts` with keyword search and filters, and `GET /posts/<id>`                    |
| Sample data       | Iman Riza Wiramdhan       | `seed.py`: sample posts and profiles for the demo                                                         |
| Join requests     | Keyla Batti Putri Anggoro | `routes_requests.py`: send a request with a message, owner accepts or rejects, slots update automatically |
| My Team           | Keyla Batti Putri Anggoro | `GET /dashboard`: my posts, my requests, request statuses, and team contacts                              |
| Landing page      | Keyla Batti Putri Anggoro | `/`: short introduction with Register and Login buttons                                                   |
| Integration       | Salva Faddil Rinaldi      | Merges Pull Requests into `main`; every member tests the others' features                                 |

## Working Rules

- Each feature has one route file (a Flask Blueprint) and one template folder, so members rarely edit the same file.
- `models.py` changes only after all three members agree, because every feature depends on it.
- Git flow: pull, create a feature branch, make small commits, open a Pull Request, and let the Git coordinator merge it.
- Secret keys and the database file stay out of Git (listed in `.gitignore`).

## Wireframes

The wireframes are drawn in Figma. Each member draws the pages of their own features, and the team agrees on colors and fonts together. The link to the Figma file will be added here.

## Checkpoint Schedule

| Checkpoint                                                      | Deadline         | Status      |
| :-------------------------------------------------------------- | :--------------- | :---------- |
| Project draft                                                   | 22 November 2026 | Complete    |
| 0. Foundation: wireframes, models, deployed skeleton            | 23 November 2026 | Not started |
| 1. Parallel feature work                                        | 24 November 2026 | Not started |
| 2. Integration and cross-testing                                | 25 November 2026 | Not started |
| 3. Final deployment, sample data, report, and presentation prep | 26 November 2026 | Not started |
| Final submission                                                | 27 November 2026 | Not started |
