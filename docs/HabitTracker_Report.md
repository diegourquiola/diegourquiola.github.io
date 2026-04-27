# Habit Tracker — Project Report

---

## Overview

Habit Tracker is a full-stack web application that lets users build daily habits, check them off each day, and track their progress through streak counters. The app was built from scratch using Python, Flask, SQLite, and vanilla JavaScript — no frameworks, no external UI libraries, no deployment server required.

The goal was to build something real: a working product with a database, a REST API, a tested backend, and a functional frontend — all connected and running locally.

---

## What the Software Does

When you open the app, you land on the **Today View** — a list of every habit you're tracking, each showing:

- A color-coded dot (your chosen color for that habit)
- The habit name
- The current streak in days
- A checkbox for today

Clicking the checkbox logs that habit as complete for today. Clicking again undoes it. The streak updates immediately.

Clicking a habit card opens the **Detail Modal**, which shows:

- Current streak and longest streak displayed as large numbers
- Full completion history (most recent first)
- Edit and Delete buttons

The **Add/Edit Modal** lets you create or update a habit with a name, optional description, and a color picker.

All data persists in a local SQLite file (`habits.db`). Nothing is sent anywhere — it all lives on your machine.

---

## Planning Phase

The project started with a written spec (`docs/HabitTracker_Plan.md`) that defined the full scope before a single line of code was written. The spec covered:

- **Tech stack decisions** — Flask for the backend, SQLite for the database (file-based, no server to manage), vanilla JS for the frontend (no React, no build step)
- **Database schema** — two tables: `habits` and `completions`, with a `UNIQUE(habit_id, completed_date)` constraint to enforce business rules at the database level
- **The streak algorithm** — the most interesting logic in the project, designed upfront before implementation
- **REST API contract** — all 7 endpoints defined with HTTP methods, routes, and expected behavior
- **Implementation phases** — backend first, then tests, then frontend, in a deliberate order so each layer was verified before building on top of it

The planning work meant that when implementation started, decisions were already made. The only job was execution.

---

## Architecture

```
habit-tracker/
├── app.py               # Flask factory — wires everything together
├── database.py          # Schema + connection helper
├── streaks.py           # Pure streak functions (no DB calls)
├── routes/
│   ├── habits.py        # CRUD: GET / POST / PUT / DELETE /api/habits
│   └── completions.py   # Toggle + stats: /api/habits/<id>/complete + /stats
├── templates/
│   └── index.html       # Single HTML shell
├── static/
│   ├── css/style.css
│   └── js/app.js        # All frontend logic
└── tests/
    ├── test_streaks.py  # 14 unit tests
    └── test_api.py      # 11 integration tests
```

**Key architectural decisions:**

1. **Single-page, API-driven frontend.** Flask serves one HTML file. After that, everything is `fetch()` calls. No full-page reloads, no Jinja templating for data — the frontend is fully decoupled from the backend.

2. **Pure functions for streak logic.** `streaks.py` takes a list of date strings and returns an integer. It has no database calls, no side effects, no dependencies. This made it trivially testable — you just call the function with inputs and check the output.

3. **Database-level constraint enforcement.** The `UNIQUE(habit_id, completed_date)` constraint means it is physically impossible to log the same habit twice on the same day. The database enforces the rule, not the application code. This is more reliable and requires zero extra validation logic.

---

## The Streak Algorithm

This was the most interesting engineering problem in the project.

A naive approach — "count all completions" — doesn't work. A habit completed 100 times over two years but not at all this month should have a streak of 0, not 100.

The correct approach is to **walk backwards through time from today**, counting only consecutive days.

```python
def current_streak(completions: list, today: str) -> int:
    dates = sorted(set(completions), reverse=True)
    if not dates:
        return 0

    cursor = today if today in dates else _prev_day(today)

    streak = 0
    for d in dates:
        if d == cursor:
            streak += 1
            cursor = _prev_day(cursor)
        elif d < cursor:
            break
    return streak
```

**The key edge case:** if it's 7am and you haven't checked in yet today, your streak shouldn't break just because today hasn't been logged. The function handles this by starting from yesterday if today has no entry — so your streak stays intact until you check off today (which adds to it) or until tomorrow morning if you forgot (which breaks it).

**Longest streak** is a separate function that scans the entire history for the longest consecutive run:

```python
def longest_streak(completions: list) -> int:
    dates = sorted(set(completions))
    if not dates:
        return 0

    longest = current = 1
    for i in range(1, len(dates)):
        if _days_between(dates[i - 1], dates[i]) == 1:
            current += 1
            longest = max(longest, current)
        else:
            current = 1
    return longest
```

---

## REST API

| Method | Route | Description |
|--------|-------|-------------|
| GET | `/api/habits` | All habits with today's status and current streak |
| POST | `/api/habits` | Create a habit |
| PUT | `/api/habits/<id>` | Update name, description, color |
| DELETE | `/api/habits/<id>` | Delete habit and all its history |
| POST | `/api/habits/<id>/complete` | Log today as complete |
| DELETE | `/api/habits/<id>/complete` | Undo today's completion |
| GET | `/api/habits/<id>/stats` | Current streak, longest streak, full history |

All endpoints return JSON. Errors return appropriate HTTP status codes (400 for bad input, 404 for not found).

---

## Testing

**25 tests total — all passing.**

### Unit Tests (14) — `tests/test_streaks.py`

These test the streak functions in complete isolation. No Flask, no database, no HTTP — just Python functions called with inputs:

- Empty history returns 0
- Single entry today returns 1
- Single entry yesterday (today not logged) — streak still alive
- Two consecutive days → streak 2
- Gap in the middle breaks the streak
- Streak of 0 when last activity was 3+ days ago
- Longer consecutive runs
- Duplicate dates are deduplicated
- Longest streak tracks the max across gaps

These tests run in milliseconds and never touch the filesystem. They are the fastest feedback loop in the project.

### Integration Tests (11) — `tests/test_api.py`

These test the full stack: HTTP request → Flask route → SQLite → response. Each test runs against a temporary database (created fresh in a temp directory, deleted after the test) so tests never interfere with each other.

Tests cover:
- Empty habit list returns `[]`
- Creating a habit returns 201 with correct fields
- Creating a habit with no name returns 400
- Updating a habit persists changes
- Deleting a habit removes it from the list
- Completing a habit returns 200
- Completing the same habit twice is idempotent (no error, no duplicate row)
- Uncompleting returns 204
- `completed_today` flag reflects the completion state
- Stats endpoint returns streak data and history
- Stats on a nonexistent habit returns 404

**Test-Driven Development** was used for the streak module: tests were written first (and verified to fail), then the implementation was written to make them pass. This forces you to think about behavior before code.

---

## Frontend

The entire frontend is a single JavaScript file (`app.js`, ~130 lines) with no framework, no build step, no `node_modules`. It does four things:

1. **Fetches data** from the API on load and after every action
2. **Renders HTML** by building strings of DOM elements and injecting them with `innerHTML`
3. **Handles events** via `addEventListener` on dynamically rendered elements
4. **Manages modals** by adding/removing a `hidden` CSS class

The HTML file is a static shell — it has the structure (header, main, two modal divs) but no data. JavaScript fills it in. This is the same pattern used by React, Vue, and every modern framework — just without the framework.

---

## What I Learned

### Backend

- **Flask blueprints** — how to split routes across multiple files and register them with the app factory. Keeps each file focused on one concern.
- **SQLite context managers** — using `with get_db() as conn` gives you automatic commit/rollback without having to manage transactions manually.
- **`PRAGMA foreign_keys = ON`** — SQLite disables foreign key enforcement by default. You have to turn it on per connection. Without this, `ON DELETE CASCADE` does nothing.
- **`row_factory = sqlite3.Row`** — turns database rows into dict-like objects so you can access columns by name (`row["id"]`) instead of index (`row[0]`).

### Algorithm Design

- **Streak counting is a backwards traversal problem**, not a counting problem. The insight that you walk backwards from today — not forwards from the first entry — is the key to getting it right.
- **Pure functions are easier to test.** Moving the streak logic out of the route and into its own module with no dependencies made it possible to write 14 tests in minutes. If the logic lived inside the Flask route, you'd need a running server to test it.
- **Database constraints as business rules.** Putting `UNIQUE(habit_id, completed_date)` in the schema means the rule is enforced everywhere — in the app, in the tests, in the database console, if you ever migrate — without any application code.

### Frontend

- **`fetch()` is the core of modern frontend.** Everything frameworks do starts with `fetch()`. Understanding how to make a request, await the response, parse JSON, and update the DOM is the foundation of all frontend development.
- **Event delegation with dynamic HTML.** When you render HTML with `innerHTML`, the old elements are replaced and old event listeners are gone. Re-attaching listeners after every render is the correct pattern.
- **XSS prevention with `escHtml()`.** Any user-provided text that goes into `innerHTML` must be escaped. If someone names their habit `<script>alert(1)</script>`, your app should display that text, not execute it.

### Software Process

- **Write the spec before the code.** Having a clear plan meant implementation was mechanical rather than exploratory. No decision paralysis mid-build.
- **TDD on the algorithmic core.** Writing tests first for `streaks.py` caught two edge cases (duplicate dates, "streak alive if today not logged") before the implementation existed. Tests written after-the-fact tend to only test the happy path.
- **Build the API before the frontend.** Verifying the backend with curl before writing any JavaScript meant that when frontend bugs appeared, the cause was always in the JS, never in the API. Separating layers eliminates variables.
- **Frequent small commits.** Every logical unit of work got its own commit. The git log tells the story of how the project was built, and you can always roll back to a working state.

---

## What I'd Add Next

The following were intentionally left out to stay in scope, but are natural next steps:

- **Weekly heatmap** — a GitHub-style grid showing the last 12 weeks of completions per habit
- **Sort by streak** — order habits by current streak so the most active ones float to the top
- **Archive instead of delete** — soft-delete so you can hide habits without losing history
- **Responsive mobile layout** — the current layout works on mobile but could be more polished
- **User accounts** — would require auth (Flask-Login or JWT), a `users` table, and filtering all queries by user ID

---

## How to Run

```bash
# Clone / navigate to project
cd HabitTracker

# Create virtual environment and install dependencies
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# Start the server
python3 app.py

# Open in browser
open http://127.0.0.1:5000

# Run tests
pytest tests/ -v
```

---

*Built with Python 3, Flask, SQLite, and vanilla JavaScript.*
