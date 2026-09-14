# AI-Study-Coach

Students usually struggle with time management, productivity, and effective study strategies. Many students rely on scheduling apps or planners that don't adapt to their unique study patterns.

This website will solve that issue by using AI to help create a study plan based on the student's schedule and can reschedule when an unexpected event occurs. Students will have their own personal study coach to help provide study strategies, study guides, an in-depth explanations on study topics. It will also track your productivity and analyze your study patterns to create the most optimal study plan for you.

A full-stack study planning platform that builds adaptive study schedules, tracks productivity, and reschedules automatically when plans change. Built with a three-person team over seven two-week sprints.
 
**Live demo:** [ai-study-coach.vercel.app](https://ai-study-coach.vercel.app) · **Stack:** React · Flask · PostgreSQL · GPT-4
 
<!-- Replace with a real screenshot before sharing this repo.
     A GIF of the calendar drag-and-drop or the Kanban board is worth
     more than any paragraph below it. -->
![Dashboard](docs/screenshots/dashboard.png)
 
---
 
## What it does
 
Students often plan with tools that don't adapt. A fixed calendar breaks the first time something runs long or an unexpected commitment appears, and rebuilding the plan by hand is the step people skip.
 
This app generates a study plan from a student's own schedule and course load, then rebuilds it when reality diverges. It tracks session data over time and surfaces patterns back to the user, and it uses the GPT-4 API for study strategies, topic explanations, and guide generation.
 
## Features
 
| | |
|---|---|
| **Adaptive scheduling** | Drag-and-drop calendar that regenerates the plan when a session moves or gets missed |
| **AI tutoring** | GPT-4 integration for study strategies, topic explanations, and generated study guides |
| **Study groups** | Collaborative groups with real-time chat |
| **Task management** | Kanban board for assignments and deadlines |
| **Productivity analytics** | Session tracking with pattern analysis surfaced back to the user |
| **Gamification** | Points and leaderboards across groups |
| **Authentication** | JWT-based auth with protected routes |
 
Nine feature modules total, built across seven sprints.
 
## Tech stack
 
**Frontend** — React, JavaScript, CSS
**Backend** — Python, Flask, SQLAlchemy ORM
**Database** — PostgreSQL, 10+ tables with enforced referential integrity
**AI** — OpenAI GPT-4 API
**Testing** — PyTest, Jest, 44% measured coverage
**CI/CD** — GitHub Actions deploying to Vercel (frontend) and Render (backend), with automated test gates and database migrations
 
## Architecture
 
```
React SPA  ──HTTP/JSON──>  Flask REST API  ──SQLAlchemy──>  PostgreSQL
                                  │
                                  └──────────>  OpenAI GPT-4 API
```
 
The schema normalizes users, study sessions, courses, tasks, groups, messages, achievements, and points into separate tables with foreign key constraints, so group membership and point totals stay consistent under concurrent writes rather than being recalculated on read.
 
## Running locally
 
**Requirements:** Python 3.x, Node 18+, PostgreSQL 14+
 
```bash
git clone https://github.com/ellis-chang/AI-Study-Coach.git
cd AI-Study-Coach
 
# Backend
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate
pip install -r requirements.txt
 
# Environment
cp .env.example .env              # then fill in the values below
 
# Database
flask db upgrade
 
# Run
python run.py                     # backend on :5000
```
 
```bash
# Frontend, in a second terminal
cd frontend
npm install
npm start                         # frontend on :3000
```
 
**Environment variables**
 
| Variable | Purpose |
|---|---|
| `DATABASE_URL` | PostgreSQL connection string |
| `OPENAI_API_KEY` | GPT-4 API access |
| `JWT_SECRET_KEY` | Token signing |
| `FLASK_ENV` | `development` or `production` |
 
<!-- Verify these against your actual config.py before publishing.
     A README with wrong setup steps is worse than none. -->
 
## Testing
 
```bash
pytest --cov                      # backend
cd frontend && npm test           # frontend
```
 
CI runs both suites on every push. The pipeline blocks merges on failing tests and applies pending database migrations before deploying.
 
## Notes on the build
 
**Team and role.** Built by three people over a 14-week schedule, delivered two weeks early. I led the project: sprint planning, the branching and code review process, the database schema, and the CI/CD pipeline.
 
**Why a normalized schema.** An earlier version stored group membership and point totals as denormalized fields, which drifted whenever two updates landed close together. Splitting them into proper relations with foreign key constraints cost more joins and removed a class of consistency bug entirely.
 
**Why test coverage was a priority.** Nine feature modules across three developers meant merges touched code nobody had written that week. Gating merges on a passing suite caught regressions in shared modules that code review missed.
 
<!-- Add a third note here if a specific bug or tradeoff comes to mind.
     Concrete decisions with reasons are the most valuable part of this file
     for anyone evaluating how you work. -->
 
## License
 
MIT
