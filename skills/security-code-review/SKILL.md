---
name: security-code-review
description: Review web application code, diffs, or pull requests for actionable security weaknesses, missing controls, insecure design, and security regressions. Review only unless remediation is requested.
---

# Security Code Review

## Procedure

1. Establish the review scope and affected code paths; examine relevant callers, middleware, configuration, and tests before reporting a finding.
2. Trace untrusted inputs, authorization decisions, sensitive data, persistence, external interactions, and exceptional paths. Consider credible abuse cases and actual deployment assumptions.
3. Evaluate applicable requirements against project code, OWASP Top 10:2025, and ASVS. Look for missing or broken server-side access control, IDOR, unsafe input handling/injection, authentication/session weaknesses, CSRF, CORS, unsafe redirects, weak cryptography, secrets, sensitive exposure, unsafe errors, logging gaps, supply chain risks, race conditions, business logic flaws, and inadequate security tests.
4. Determine realistic preconditions, impact, and confidence. Distinguish confirmed weaknesses from risks needing investigation; do not invent exploitability, line numbers, or identifiers.
5. Prioritize by likely impact and exploitability in context. Use Critical, High, Medium, Low, or Informational severity and High, Medium, or Low confidence; explain uncertainty. Do not invent CVSS scores.
6. Provide concrete remediation and a regression test. Avoid duplicate findings, stylistic noise, and unrelated changes. Report no findings only within the inspected scope, not as a security guarantee.

## Finding template

- **Title:** concise weakness
- **Severity / confidence:** reasoned assessment
- **Location:** actual file and line(s), or identified symbol if lines unavailable
- **Status:** confirmed weakness or potential risk requiring verification
- **Evidence and preconditions:** relevant code behavior; redact sensitive values
- **Impact:** affected assets, identities, or operations
- **Classification:** verified OWASP/CWE/ASVS reference when applicable
- **Remediation:** actionable minimal fix
- **Regression test:** permitted and denied behavior as appropriate

Report findings in severity order, followed by review scope, limitations, and unverified assumptions. Do not modify code unless authorized.

Apply the agent’s mandatory safeguards and applicable security policies. Use the [core policy](../../instructions/secure-coding.instructions.md) and [web application policy](../../instructions/web-application-security.instructions.md) from this installation when relevant; read them if not already loaded. Stay within the authorized task. Do not claim unavailable guidance or checks were used.
