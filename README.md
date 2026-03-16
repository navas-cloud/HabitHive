# HabitHive — Interactive Habit Tracking Platform

![Python](https://img.shields.io/badge/Python-3.10-3776AB?logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Framework-Flask-000000?logo=flask&logoColor=white)
![SQLServer](https://img.shields.io/badge/DB-SQL_Server-CC2927?logo=microsoftsqlserver&logoColor=white)
![Bootstrap](https://img.shields.io/badge/UI-Bootstrap_5-7952B3?logo=bootstrap&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

> A web-based habit tracking application that recommends habits based on BMI and age, tracks daily completions, and visualizes progress through interactive charts.

> 🤝 Built as a team project with [Priyanka R P](https://github.com/PriyankaRS17)

---

## Problem Statement

Most habit trackers treat all users the same. HabitHive takes a different approach — it uses the user's weight, height, and age to calculate BMI and recommend relevant habits automatically. Combined with daily completion tracking and visual stats, it gives users a personalized, data-driven habit-building experience.

---

## Features

| Feature | Description |
|---|---|
| Register / Login | Secure auth with Werkzeug password hashing |
| Personalized Habit Recommendations | Recommends habits based on BMI category (Underweight / Normal / Overweight / Obese) and age group (Teen / Young Adult / Adult / Senior) |
| Add Custom Habits | User types their own habit name or selects from a dropdown |
| Habit Configuration | Set frequency (days/week), duration (hours), and preferred time (Morning / Afternoon / Night) |
| Daily Completion Tracking | Mark habits complete per day — duplicate completions blocked server-side |
| Delete Habits | Remove habits and all associated completion records |
| Hive Stats Dashboard | Visual charts — daily completion trend, remaining habit targets, preferred time distribution |
| Blog Section | 3 built-in articles on habit building, morning routines, and mindfulness with related article suggestions |
| Health Profile | User stores weight, height, age, gender — persisted on User model, updates on every habit form submit |

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Python 3.x |
| Backend Framework | Flask |
| Frontend | Bootstrap 5, JavaScript, HTML, CSS |
| Database | Microsoft SQL Server (via `mssql+pyodbc`) |
| ORM | Flask-SQLAlchemy |
| Authentication | Flask-Login + Werkzeug password hashing |
| Forms | Flask-WTF + WTForms |
| Charts | JavaScript (chart data passed from Flask via Jinja2) |

---

## Screenshots

  Dashboard
<img width="1898" height="845" alt="image" src="https://github.com/user-attachments/assets/158e0794-7244-47f9-b8a1-f51b809ccaae" />

  Login
<img width="1916" height="880" alt="image" src="https://github.com/user-attachments/assets/ef0ab300-881b-47c0-9b32-c90cabfd76ca" />

  Habits
<img width="1891" height="875" alt="image" src="https://github.com/user-attachments/assets/22a7c755-3569-4a80-9ad8-c76ac3c8c7a9" />

  Hive Stats
<img width="1896" height="881" alt="image" src="https://github.com/user-attachments/assets/93bfc5a4-a02c-4701-bb6e-660bfec07717" />

---

## Project Structure

```
Habit Tracker/
│
├── app.py                  # Main Flask app — all routes and business logic
├── models.py               # User, Habit, HabitCompletion models
├── forms.py                # LoginForm, RegisterForm, HabitForm (Flask-WTF)
│
├── templates/
│   ├── dashboard.html      # Home page — articles feed
│   ├── login.html
│   ├── register.html
│   ├── habit.html          # Add habits + BMI recommendations + habit list
│   ├── hive_stats.html     # Charts — completion trend, targets, time distribution
│   └── blog_detail.html    # Individual blog article with related articles
│
└── static/
    ├── style.css
    ├── script.js
    └── images/
        ├── login-img.jpg
        └── sunset.jpg
```

---

## Database Schema

```
Users
──────────────────────────
id (PK)
username (unique)
password (hashed — Werkzeug)
date_created
weight (Float, nullable)
height (Float, nullable)
age (Integer, nullable)
gender (String, nullable)


Habits
──────────────────────────
id (PK)
user_id (FK → Users)
habit_name
frequency          ← target completions per duration window
duration           ← total habit duration in hours
preferred_time     ← Morning / Afternoon / Night
date_created


HabitCompletions
──────────────────────────
id (PK)
habit_id (FK → Habits)
date
completed (Boolean)
```

---

## BMI-Based Recommendation Logic

```python
# BMI Categories → Recommended Habits
Underweight (BMI < 18.5)  →  Strength Training, Balanced Meals
Normal (18.5–24.9)        →  No auto-recommendation
Overweight (25–29.9)      →  Cardio, Drink Water, Meditation
Obese (BMI ≥ 30)          →  Cardio, Drink Water, Meditation

# Age Categories → Recommended Habits
Child/Teen  (< 18)        →  Story Reading, Homework Routine
Adult       (18–55)       →  No auto-recommendation
Senior      (> 55)        →  Gentle Walk, Memory Games
```

Recommendations from both BMI and age are combined and displayed on the habit page.

---

## Hive Stats — Chart Data

The `/hive-stats` route computes and passes three datasets to the frontend:

| Chart | Data | Description |
|---|---|---|
| Daily Completion Trend | Date vs completion count | Shows which days had the most habit completions |
| Remaining Targets | Habit name vs remaining count | How many completions still needed before duration ends |
| Time Distribution | Morning / Afternoon / Night vs count | Which time slots have pending habits today |

---

## Routes

| Method | Route | Auth | Description |
|---|---|---|---|
| GET/POST | `/login` | Public | Login |
| GET/POST | `/register` | Public | Register |
| GET | `/` | Public | Dashboard — articles feed |
| GET | `/dashboard` | Public | Dashboard with current user context |
| GET/POST | `/habit` | Required | Add habits, view list, get BMI recommendations |
| POST | `/complete-habit/<id>` | Required | Mark habit complete for today (JSON API) |
| DELETE | `/delete-habit/<id>` | Required | Delete habit and all completions (JSON API) |
| GET | `/hive-stats` | Required | View completion charts |
| GET | `/blog/<id>` | Public | Read individual blog article |
| GET | `/logout` | Required | Logout |

---

## Getting Started

### Prerequisites

```bash
Python 3.8+
pip
Microsoft SQL Server (local or remote instance)
ODBC Driver 18 for SQL Server
```

### Installation

```bash
# Clone the repo
git clone https://github.com/PriyankaRS17/habithive.git
cd habithive/Habit\ Tracker

# Install dependencies
pip install -r requirements.txt
```

### Configure Database

In `app.py`, update the connection string to match your SQL Server instance:

```python
app.config['SQLALCHEMY_DATABASE_URI'] = (
    "mssql+pyodbc://@YOUR_SERVER\\SQLEXPRESS/HabitHive?"
    "driver=ODBC+Driver+18+for+SQL+Server&trusted_connection=yes&encrypt=no"
)
```

### Run the App

```bash
python app.py
```

Open **http://127.0.0.1:5000** — database tables are auto-created on first run via `db.create_all()`.

---

## Key Engineering Decisions

- **Health profile on the User model** — weight, height, age, gender stored directly on `User`; BMI recommendations compute from `current_user` with no extra join
- **Server-side duplicate completion guard** — `/complete-habit` checks for an existing `HabitCompletion` with today's date before inserting; prevents double-counting without any client-side state
- **JSON API for complete and delete** — both actions use `jsonify` responses consumed by JavaScript, keeping the habit page interactive without full page reloads
- **Remaining target scoped to duration window** — stats compute remaining completions only within the habit's active window (`date_created` to `date_created + duration days`), not lifetime totals
- **BMI + age as additive recommendation lists** — both sets computed independently and merged; a Senior with Overweight BMI gets recommendations from both categories

---

## What I'd Improve Next

- [ ] Migrate from SQL Server to PostgreSQL for easier cross-platform deployment
- [ ] Add streak tracking — count consecutive days a habit was completed
- [ ] Add habit edit functionality (currently add and delete only)
- [ ] Write pytest unit tests for BMI logic and completion duplicate guard
- [ ] Dockerize with `docker-compose` for portable setup
- [ ] Deploy to AWS / Render

---

## Authors

**Mohammed Navas A** · [LinkedIn](https://linkedin.com/in/mohammed-navas-a-) · [GitHub](https://github.com/navas-cloud) · navash.a.v012@gmail.com

**Priyanka R P** · [LinkedIn](https://www.linkedin.com/in/priyanka-rp) · [GitHub](https://github.com/PriyankaRS17) · priyankapremnath17@gmail.com
