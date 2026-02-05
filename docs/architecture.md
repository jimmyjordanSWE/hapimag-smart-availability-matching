# Architecture

## Objective

Build a secure staff copilot for hospitality operations that returns grounded answers with citations.

## System components

- API service: receives requests and enforces authn/authz
- Retrieval service: embeds, indexes, and retrieves policy context
- LLM orchestration: prompt templates and citation formatting
- Guardrails: input/output checks and policy enforcement
- Observability: structured logs, traces, and redaction

## Data flow

1. Staff user sends question.
2. API validates session and role permissions.
3. Retrieval fetches allowed context.
4. Guardrails inspect input and context.
5. LLM generates answer with citations.
6. Output filter checks policy and sensitive data leaks.
7. Response + telemetry are stored with redacted logs.

## Non-goals

- No autonomous booking changes in MVP.
- No real personal data in MVP dataset.

