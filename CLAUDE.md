# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Run the app
py app.py                        # starts Flask dev server on port 5001

# Run all tests
venv/Scripts/pytest

# Run a single test file
venv/Scripts/pytest tests/test_auth.py

# Run a single test by name
venv/Scripts/pytest -k "test_login"

# Install dependencies
venv/Scripts/pip install -r requirements.txt
```

Activate the venv first (Git Bash): `source venv/Scripts/activate`

## Architecture

This is a server-rendered Flask app with SQLite. All routes live in `app.py`. The `database/` package is the only abstraction layer — it exposes three functions (`get_db`, `init_db`, `seed_db`) that `app.py` imports directly. There is no ORM.

**Request flow:**
1. Flask route in `app.py` calls `get_db()` from `database/db.py` to get a SQLite connection
2. Route executes raw SQL and passes results to `render_template()`
3. Jinja2 template extends `templates/base.html` and fills `{% block content %}`

**Template inheritance:** Every page extends `base.html`, which provides the sticky navbar, footer, Google Fonts (DM Sans + DM Serif Display), and `main.js`. Pages override `{% block title %}`, `{% block content %}`, and optionally `{% block scripts %}`.

**CSS design tokens:** All colours, fonts, spacing, and radii are defined as CSS custom properties in the `:root` block at the top of `style.css`. Use these variables (e.g. `var(--accent)`, `var(--ink)`) rather than hardcoded values when adding new styles.

## Planned feature steps (student exercise)

The app is scaffolded for a guided build. Placeholder routes in `app.py` map to these steps:
- **Step 1** — `database/db.py`: implement `get_db`, `init_db`, `seed_db`
- **Step 3** — `/logout`: session-based logout
- **Step 4** — `/profile`: user profile page
- **Steps 5–6** — auth logic for `/register` and `/login` (POST handlers, password hashing)
- **Steps 7–9** — `/expenses/add`, `/expenses/<id>/edit`, `/expenses/<id>/delete`
