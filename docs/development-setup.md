# Development Setup

## Prerequisites

| Tool                   | Version                               |
| :--------------------- | :------------------------------------ |
| Python                 | 3.10 or newer                         |
| pip                    | Bundled with Python                   |
| Git                    | Any recent version                    |
| GitHub account         | Access to the TemuTim repository      |
| PythonAnywhere account | Free plan, needed only for deployment |

## Project Structure

```text
temutim/
├── app.py
├── extensions.py
├── models.py
├── routes_auth.py
├── routes_profile.py
├── routes_posts.py
├── routes_requests.py
├── seed.py
├── requirements.txt
├── templates/
├── static/
├── docs/
└── .gitignore
```

`templates/` holds `base.html` and one folder per feature. `static/` holds CSS and images. The database file is created in `instance/` at the first run.

## Local Commands

```bash
git clone https://github.com/salvafaddil/TemuTim.git
cd TemuTim
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
python seed.py                  # optional: load sample data
flask --app app run --debug
```

The Flask server runs on `http://127.0.0.1:5000`. `--debug` restarts the server on file changes and shows error pages in the browser. The SQLite database is stored at `instance/temutim.db` and is created automatically on first run. To start over with an empty database, stop the server and delete that file.

## Git Workflow

```bash
git pull
git checkout -b feature/<name>
git add .
git commit -m "Describe the change"
git push -u origin feature/<name>
```

Open a Pull Request on GitHub. The Git coordinator, Salva Faddil Rinaldi, merges it into `main`. Each member works on their own feature branch and does not commit to `main` directly.

| Path        | Reason                        |
| :---------- | :---------------------------- |
| `instance/` | Holds the local database file |
| `.env`      | May hold the secret key       |
| `venv/`     | Local virtual environment     |

These paths are listed in `.gitignore`. Never commit a secret key or a database file.

## Deployment

TemuTim is hosted on a PythonAnywhere free account.

| Setting  | Value                                                                             |
| :------- | :-------------------------------------------------------------------------------- |
| Host     | PythonAnywhere free account                                                       |
| Site     | `https://<username>.pythonanywhere.com` (to be set)                               |
| Update   | Run `git pull` in a PythonAnywhere Bash console, then press Reload on the Web tab |
| Database | SQLite file on PythonAnywhere's persistent file system                            |

First-time setup:

1. In a Bash console, clone the repository and create a virtual environment.
2. Run `pip install -r requirements.txt` inside the virtual environment.
3. On the Web tab, add a new Flask web app and set the source code folder and the virtual environment path.
4. Point the WSGI file at the Flask app created in `app.py`.
5. Set the `SECRET_KEY` environment variable.
6. Press Reload and open the site.

Free account limits to keep in mind (check them again at deploy time, since plans can change):

| Limit                          | Effect on the project                              |
| :----------------------------- | :------------------------------------------------- |
| One web app                    | The whole site runs as a single Flask app.         |
| 1-month expiry                 | The web app must be extended manually every month. |
| Restricted outbound internet   | The app must not depend on external APIs.          |
| No free MySQL for new accounts | SQLite is used instead.                            |

SQLite allows one write at a time. This is enough for a course project, but a busy site would need to move to PostgreSQL.

## Environment Variables

| Variable     | Default | Used by                                                    |
| :----------- | :------ | :--------------------------------------------------------- |
| `SECRET_KEY` | None    | Flask sessions, Flask-Login, and Flask-WTF CSRF protection |

Set `SECRET_KEY` in the shell before running locally, and in the web app settings on PythonAnywhere. The database path needs no variable because SQLite uses `instance/temutim.db`.

(Hosting URL and the first deployment: TBD in Phase 0.)

