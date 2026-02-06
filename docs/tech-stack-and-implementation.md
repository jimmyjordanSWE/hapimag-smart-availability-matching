# Tech Stack and Implementation Details

## Scope

This service handles sold-out booking recovery for the booking frontend.

Primary endpoints:
- `POST /match`
- `POST /feedback`
- `GET /health`

## Chosen stack (single path)

### API
- Python 3.11
- FastAPI
- Pydantic
- Uvicorn/Gunicorn

### Data layer
- PostgreSQL
- SQLAlchemy + Alembic
- Optional `pgvector` for similarity features (not required for MVP)

### Ranking and reasoning
- Deterministic rule engine for hard constraints
- Weighted scoring engine for soft ranking
- Templated explanation generator
- Waitlist likelihood estimator (heuristic first)

### Security and controls
- JWT/API key auth (depending on integration context)
- Request schema validation and strict input bounds
- Redacted structured logs
- Allow-list based outbound network policy in production

### Observability
- JSON logging
- OpenTelemetry traces
- Metrics: latency, error rate, match success rate, fallback rate

### Deployment
- Dockerized API
- Managed Postgres (Render/Neon/Supabase)
- Deploy API on Render or Fly.io
- Platform ingress is sufficient for MVP; add Nginx only if hardening requires it

## Performance targets (MVP)

- `P95 /match latency <= 800ms` without LLM explanation
- `P95 /match latency <= 2.5s` with optional LLM explanation
- Error rate `< 1%` under pilot load
- Health endpoint success rate `>= 99.9%`

## Feature order

1. Data schema + synthetic seed data
2. `POST /match` with hard filters
3. Weighted ranking + top 3 output
4. Waitlist score + explanation strings
5. `POST /feedback` persistence
6. Eval script + benchmark report
7. Observability and rollout guardrails

## Data policy

- Source-of-truth is synthetic inventory, cancellation, and preference data.
- No real customer data in this repository.
- External open datasets are used only for scenario stress testing.

