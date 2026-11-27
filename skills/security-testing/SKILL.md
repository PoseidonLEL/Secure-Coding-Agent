---
name: security-testing
description: Design and run meaningful functional and security regression tests for web application controls, negative cases, access boundaries, abuse prevention, and exceptional behavior.
---

# Security Testing

## Procedure

1. Identify intended security properties, affected trust boundaries, and existing test framework/fixtures. Reuse project conventions and run tests only in an authorized isolated environment.
2. Select applicable positive, negative, and boundary cases from the matrix below. Prefer integration tests at the boundary that enforces the control, supplemented by unit tests where useful.
3. Use deterministic synthetic fixtures and separate principals/tenants. Do not mock or bypass the security control under test. Where practical, demonstrate that the regression test detects the original weakness.
4. Assert both response behavior and absence of unauthorized data disclosure or side effects. Verify relevant state, storage, logs, and transactions without exposing secrets.
5. Run relevant tests using verified project commands. Do not weaken assertions or remove tests to obtain a pass. Record actual results and blockers; hand off to security-verification for final checks.

## Applicable test matrix

| Property | Example test cases |
| --- | --- |
| Authentication / session | Unauthenticated, expired, invalid, revoked or wrong-purpose credentials; legitimate access. |
| Authorization | Horizontal/vertical escalation, tenant boundaries, object/property/list/export access; permitted role. |
| Validation / injection | Invalid types, ranges, sizes, malformed input, unexpected properties, safe treatment of untrusted data. |
| Sensitive data | Restricted response fields, errors, logs, caches and downloads; no cross-user leakage. |
| Workflow integrity | Replay, idempotency conflicts, token reuse, concurrency, rollback, unauthorized state changes. |
| Abuse / availability | Relevant rate limits, pagination, upload/parser limits, bounded failure and retry behavior. |
| Browser / API | CSRF, origin checks, safe redirects, encoding, SSRF restrictions, webhook verification where applicable. |
| Configuration | Production-safe defaults, missing security-critical settings, disabled mock/debug behavior. |

Keep coverage proportional. Do not claim a passing test proves every attack variant is prevented.

Apply the agent’s mandatory safeguards and applicable security policies. Use the [core policy](../../instructions/secure-coding.instructions.md) and [web application policy](../../instructions/web-application-security.instructions.md) from this installation when relevant; read them if not already loaded. Stay within the authorized task. Do not claim unavailable guidance or checks were used.
