# Deployment Rollout and Multilingual Plan

## Objective

Roll out safely in Swiss pilot sites first, then expand across Europe with language and reliability controls.

## Rollout model

### Phase 0 - Internal validation

- Run with synthetic data only.
- Validate security controls, eval quality, and failure behavior.
- Define go/no-go SLOs.

### Phase 1 - Swiss pilot (limited properties)

- Start with a small number of Swiss resorts.
- Initial languages: German, French, Italian, English.
- Limit scope to staff-assist Q&A (read-only guidance).
- Track metrics daily and capture policy gaps.

### Phase 2 - Swiss expansion

- Expand to more Swiss sites after stable pilot metrics.
- Add manager workflows and stronger escalation playbooks.
- Harden observability and incident response.

### Phase 3 - Europe rollout

- Expand country-by-country with language packs and localized policy content.
- Add Spanish, Portuguese, Dutch, and other high-demand locales.
- Keep same security baseline and run localized eval suites before each launch.

## Multilingual strategy

- Use multilingual embeddings for cross-language retrieval.
- Index documents with locale metadata and business unit tags.
- Prefer in-language answers; fallback to approved pivot language only when needed.
- Maintain translation QA process for policy-critical content.
- Report eval metrics by language to avoid hidden regressions.

## Capacity planning terms (for interviews)

- Throughput: sustained requests/minute supported.
- Concurrency: active sessions handled at peak.
- P95 latency: 95th percentile response time.
- Error budget: allowed failure rate before release pause.
- MTTR: mean time to recover from incidents.

## Pilot success criteria (example)

- Grounded answer rate >= 90%
- Policy citation coverage >= 95%
- P95 latency <= 2.5s
- Prompt-injection block rate >= 99% on test suite
- No critical privacy leakage incidents

## Risk controls during expansion

- Feature flags by country/site
- Canary rollout for each new language
- Kill switch for unsafe behavior
- Human escalation path for uncertain answers

