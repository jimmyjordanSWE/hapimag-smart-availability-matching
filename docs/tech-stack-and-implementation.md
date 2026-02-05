# Tech Stack and Implementation Details

## TL;DR decisions

- Do **not** train a model from scratch for this project.
- Do **not** use Kaggle as a runtime platform.
- Build a secure RAG system with strong controls and optional self-hosted inference.

## Why not train from scratch

- Training is expensive, slow, and weak signal for this role.
- Hapimag value is in secure integration, retrieval quality, safety, and operations.
- A strong retrieval + guardrails architecture is the senior signal.

## Recommended stack (pragmatic)

### App/API

- Python 3.11
- FastAPI
- Pydantic
- Uvicorn

### Retrieval

- LangChain or LlamaIndex (thin orchestration only)
- pgvector (PostgreSQL) for vector search
- `bge-small-en-v1.5` or `multilingual-e5-base` embeddings

### LLM inference (privacy-first options)

Option A (best demo control):
- Self-hosted model via Ollama or vLLM
- Suggested model class: 7B-14B instruct model

Option B (faster dev, still enterprise-acceptable if configured):
- Azure OpenAI / enterprise API with:
  - no training on customer data
  - strict data retention policy
  - regional controls

### Security and controls

- JWT auth + role-based authorization
- Retrieval ACL by role/document class
- Input/output guardrails
- PII redaction in logs
- Secret management via environment + vault pattern

### Observability

- Structured JSON logs
- OpenTelemetry traces
- Prometheus metrics
- Basic eval report (groundedness, latency, refusal safety)

### Deployment

- Docker + docker-compose for local demo
- Kubernetes-ready manifests (optional stretch)

## Suggested implementation plan (feature order)

1. Ingest synthetic policy docs and build vector index.
2. Add `/ask` endpoint with citations.
3. Add role-based retrieval permissions.
4. Add prompt-injection + sensitive-output filters.
5. Add `/feedback` and eval harness.
6. Add logging/tracing and redaction checks.

## Data policy for this demo

- Source-of-truth: synthetic policy/SOP documents only.
- Open datasets: evaluation and scenario generation only.
- No real guest data in repository.

## Answer to "Can we use ChatGPT API?"

Yes, but with enterprise controls and clear boundaries.  
For strongest "company-secrets-safe" posture in interviews, self-hosted inference (Ollama/vLLM) is easier to defend.

