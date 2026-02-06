# Hapimag Smart Availability Matching

Production-minded AI demo for sold-out booking recovery in hospitality.

## Why this exists

This project demonstrates a practical system for:

- recovering failed booking intent,
- recommending high-fit alternatives with transparent reasons,
- improving inventory usage with controlled rollout and guardrails.

## MVP features

- `POST /match` endpoint for sold-out recovery
- Top-3 ranked alternatives with explanation strings
- Waitlist likelihood score (0-100)
- `POST /feedback` loop for model/ranker tuning
- Evaluation report for recovery and ranking quality

## Repo map

```text
docs/      architecture, threat model, compliance notes, runbook
app/       API and ranking modules
data/      synthetic inventory and policy context
tests/     unit, scoring, and safety tests
demo/      interview walkthrough script
```

## Technical design docs

- `docs/tech-stack-and-implementation.md`
- `docs/deployment-rollout-and-multilingual.md`
- `docs/smart-availability-step-by-step.md`
- `docs/architecture.md`
- `docs/threat-model.md`
- `docs/compliance-notes.md`

## Quick start

Build and run instructions are defined in:
- `docs/smart-availability-step-by-step.md`
