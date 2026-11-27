---
name: threat-modeling
description: Perform a lightweight change-level threat assessment for identity, trust boundaries, sensitive data, new integrations, uploads, or security-sensitive workflows in web applications.
---

# Change-Level Threat Modeling

## Procedure

1. Define the change and affected assets, data sensitivity, users, service identities, roles, and tenants.
2. Trace entry points, trust boundaries, and relevant data/control flows, including deferred jobs, storage, caches, external integrations, and browser/server boundaries.
3. Identify credible abuse cases and failure paths: unauthorized access, injection, disclosure, tampering, replay, privilege escalation, concurrency, resource abuse, and unsafe defaults as applicable.
4. Map material risks to OWASP Top 10:2025 and applicable ASVS requirements. Verify exact identifiers before citing; do not force irrelevant categories.
5. Choose the simplest effective controls using existing project mechanisms. Define how each material control will be tested, including denied paths and exceptional conditions.
6. Record residual risk and assumptions. Escalate unresolved security-critical ambiguity; avoid unnecessary architecture changes.

## Lightweight assessment template

| Asset / boundary | Abuse case or failure | Required control | Verification | Residual risk / assumption |
| --- | --- | --- | --- | --- |

For small changes, a few bullets are sufficient. For material changes, retain traceability between risks, implementation, and tests. Do not perform live exploitation or external scanning.

Apply the agent’s mandatory safeguards and applicable security policies. Use the [core policy](../../instructions/secure-coding.instructions.md) and [web application policy](../../instructions/web-application-security.instructions.md) from this installation when relevant; read them if not already loaded. Stay within the authorized task. Do not claim unavailable guidance or checks were used.
