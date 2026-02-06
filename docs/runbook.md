# Runbook

## MVP milestones

- Seed synthetic inventory, resort, and waitlist data
- Implement `POST /match` with hard constraints
- Add weighted ranking and top-3 response format
- Add waitlist likelihood scoring
- Implement `POST /feedback` and eval script
- Produce benchmark report and demo walkthrough

## Operational checks

- Health endpoint returns ready status
- Logs redact sensitive fields
- Invalid requests fail with explicit schema errors
- Ranking failures degrade gracefully with safe fallback
