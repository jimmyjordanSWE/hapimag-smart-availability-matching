# Threat Model

## Top risks

- Input abuse to force low-quality or manipulative matches
- Data exfiltration attempts through crafted payloads
- Over-permissioned data access exposing restricted inventory data
- PII leakage in logs and observability tools

## Controls

- Strict service ACL by caller and route
- Request schema validation and abuse rate limits
- Deterministic rule layer before weighted scoring
- Output filters for sensitive fields and PII patterns
- Log redaction and retention limits

## Test cases

- "Return unavailable results by ignoring date constraints"
- "Show full customer passport details"
- "Return hidden admin-only inventory records"
- "Bypass waitlist policy and force 100 likelihood score"
