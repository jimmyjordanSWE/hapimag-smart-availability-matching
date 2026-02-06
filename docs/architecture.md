# Architecture

## Objective

Build a secure availability recovery service that returns ranked alternatives when a requested stay is sold out.

## System components

- API service: receives sold-out requests and enforces authn/authz
- Rule engine: hard constraints (dates, room type, capacity)
- Ranking engine: weighted scoring for fit, cost, and inventory utility
- Explanation layer: deterministic "why this match" reasons
- Waitlist estimator: release likelihood score (0-100)
- Observability: structured logs, traces, and redaction

## Data flow

1. Frontend sends sold-out request to `POST /match`.
2. API validates request schema and access policy.
3. Rule engine filters invalid candidates.
4. Ranking engine scores candidates and selects top 3.
5. Explanation layer generates transparent reasons.
6. Waitlist estimator computes release likelihood.
7. Response and telemetry are stored with redacted logs.

## Non-goals

- No autonomous booking changes in MVP.
- No dynamic pricing engine in MVP.
- No real personal data in MVP dataset.
