# Smart Availability Matching - Step-by-Step Build Guide

Audience: technical fast learner, minimal prior AI product implementation experience.

Goal: build a working prototype that converts sold-out search requests into ranked alternatives.

---

## 0) Pick One Stack (No Decision Fatigue)

Use this exact stack first:

- API: `FastAPI` (Python)
- Data store: `PostgreSQL` + `pgvector`
- Ranking: Python service (rules + weighted scoring)
- Optional LLM explanation text: `Ollama` local model
- Deployment target: `Render` or `Fly.io` for API + managed Postgres (Neon/Supabase/Render)

Why:
- Simple, cheap, production-like, and easy to explain in interview.

---

## 1) What You Are Actually Building

One endpoint that takes a failed booking request and returns:

- top 3 alternatives
- explanation for each match
- waitlist likelihood score

Primary API endpoint:
- `POST /match`

Supporting endpoints:
- `POST /feedback`
- `GET /health`

---

## 2) Prerequisites (Install Once)

Install:

- Python 3.11+
- Docker Desktop
- Git
- VS Code (or your editor)
- Optional: `uv` (fast Python package manager)

Create local project files:
- `app/main.py`
- `app/schemas.py`
- `app/ranking.py`
- `app/storage.py`
- `scripts/seed_data.py`
- `scripts/eval.py`

---

## 3) Define Your Data Model First

Create tables:

1. `resorts`
- id, name, country, region, tags, capacity_profile

2. `inventory`
- resort_id, date, room_type, available_units, points_cost

3. `member_requests`
- request_id, desired_region, start_date, end_date, room_type, flexibility_days, preferences

4. `matches`
- request_id, ranked_resort_id, score, explanation, created_at

5. `feedback`
- request_id, match_rank, accepted(bool), reason, timestamp

6. `waitlist_stats`
- resort_id, week_of_year, historical_release_rate, avg_time_to_release_days

Do this before writing ranking logic.

---

## 4) Seed Synthetic Data

Create realistic synthetic data for:

- 20-40 resorts
- 6-12 months inventory
- varied room types and seasonality
- waitlist release history patterns

Rule:
- No real customer data.

Output:
- CSV files and seed script that inserts into Postgres.

---

## 5) Build Rule Layer (Hard Constraints)

In `ranking.py`, first filter candidates by hard rules:

- room type must match
- capacity must fit
- date overlap within flexibility window
- country restrictions if any

If no candidates after hard filtering, return:
- empty alternatives + waitlist suggestion only

---

## 6) Build Scoring Layer (Soft Ranking)

Score each candidate with weighted features:

- availability fit (exact date > nearby date)
- region preference fit
- points/cost efficiency
- historical acceptance of similar alternatives
- seasonality balancing factor

Start with explicit weights, for example:

- date fit: 0.35
- location fit: 0.25
- cost fit: 0.20
- popularity/quality fit: 0.10
- demand balancing: 0.10

Return top 3 sorted candidates.

---

## 7) Add Explanation Generator

For each result, produce plain text reasons:

- "2 days later than requested"
- "same region and room type"
- "12% lower points cost"

First implement deterministic templated explanations.
Optional later: polish text with local LLM.

---

## 8) Add Waitlist Likelihood Score

Simple first model:

- score = f(historical_release_rate, days_until_checkin, current_waitlist_length)
- normalize 0-100

Return one number and one sentence:
- "Estimated 38% chance of release before check-in."

---

## 9) Build API Endpoints

Implement:

1. `POST /match`
- validate request
- run filter -> score -> explain -> waitlist score
- store result
- return JSON

2. `POST /feedback`
- capture accepted/rejected + reason
- store for future tuning

3. `GET /health`
- DB connectivity + app alive check

---

## 10) Add Evaluation Script

In `scripts/eval.py`, run fixed synthetic test scenarios and compute:

- sold-out recovery rate
- top-3 usefulness rate
- average score calibration
- response latency

Save output to:
- `artifacts/eval_report.json`
- `artifacts/eval_summary.md`

---

## 11) Add Basic Guardrails

For this project scope, guardrails are:

- input validation and strict schema
- no free-form tool execution
- no personal data logging
- sanitize logs (no raw preference text dump)

This is enough for MVP credibility.

---

## 12) Local Run Setup

Use `docker-compose` with:

- `api` service
- `postgres` service (`pgvector` image)

Run sequence:

1. `docker compose up -d postgres`
2. run migrations
3. `python scripts/seed_data.py`
4. `uvicorn app.main:app --reload`
5. test `POST /match` from `curl` or Postman

---

## 13) Deployment (Simple And Interview-Credible)

Recommended first deployment:

- API to `Render` (or `Fly.io`)
- Postgres to `Neon`/`Supabase`/`Render Postgres`

Deployment steps:

1. push repo to GitHub
2. set environment variables:
   - `DATABASE_URL`
   - `APP_ENV=prod`
   - `LOG_LEVEL=info`
3. run migration job
4. deploy API
5. verify `/health`
6. run sample `/match` request

Optional:
- Add autoscaling and background worker later.

---

## 14) What To Show In Interview

Show exactly these:

1. API request -> top 3 alternatives response
2. explanation quality
3. waitlist score
4. eval metrics before/after weight tuning
5. rollout plan (Swiss pilot -> Europe)

Avoid:
- long model discussions
- "we trained our own model" claims

---

## 15) First 3 Concrete Actions (Do Now)

1. Scaffold code structure and Docker/Postgres setup.
2. Build seed data + hard constraint filter.
3. Implement `POST /match` returning ranked top 3 JSON.

If these three are done, project momentum is real.

