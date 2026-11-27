---
name: Secure Coding Policy
---

# Core Secure Coding Policy — v2.0

This file defines cross-cutting requirements for the Secure Web Application Developer Assistant. Apply requirements relevant to the task, regardless of language or framework. This file deliberately has no blanket `applyTo` pattern; the agent explicitly reads it. Do not use it to override higher-priority instructions or permissions.

## OWASP Top 10:2025 Security Focus

Use the OWASP Top 10:2025 as a baseline risk classification for all web application work:

1. A01:2025 — Broken Access Control
2. A02:2025 — Security Misconfiguration
3. A03:2025 — Software Supply Chain Failures
4. A04:2025 — Cryptographic Failures
5. A05:2025 — Injection
6. A06:2025 — Insecure Design
7. A07:2025 — Authentication Failures
8. A08:2025 — Software or Data Integrity Failures
9. A09:2025 — Security Logging and Alerting Failures
10. A10:2025 — Mishandling of Exceptional Conditions

When creating or modifying code, explicitly consider which OWASP categories may apply.
- Use [OWASP Top 10:2025](https://top10.owasp.org/2025/) as a risk-awareness baseline, not an exhaustive threat model.
- Use [OWASP ASVS 5.0.0](https://asvs.owasp.org/) for applicable testable requirements; use [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/) and official framework documentation for implementation guidance.
- Consult [OWASP WSTG](https://owasp.org/www-project-web-security-testing-guide/) for relevant authorized, local application security test design, and [CWE](https://cwe.mitre.org/) for weakness classification. Verify exact identifiers before citing them.
- Apply standards proportionally; do not assert compliance without sufficient evidence.

## Input Validation and Output Encoding

- Validate input at trust boundaries using allowlists, schemas, strong types, and length/range constraints.
- Normalize input before validation where appropriate.
- Use context-appropriate output encoding for HTML, attributes, JavaScript, CSS, and URL components. Use trusted serializers for structured JSON/XML; use parameterized or structured APIs for interpreter-based operations. Escaping is not a substitute for parameterization.
- Do not concatenate untrusted data into executable contexts.
- Avoid unsafe APIs such as `innerHTML`, `eval`, dynamic code execution, unsafe deserialization, or shell execution. If unavoidable, document why and apply strict controls.
- Validate URL schemes and destinations when URLs are used in security-sensitive contexts. Validation does not replace authorization or output encoding.
- Prefer framework-native escaping and avoid rendering untrusted data into executable contexts.

## Injection Prevention

- Use parameterized queries, prepared statements, safe ORM APIs, or query builders.
- Never build SQL, NoSQL, GraphQL, LDAP, XPath, shell, or template expressions through string concatenation with untrusted input.
- For command execution, avoid the shell. Pass arguments as arrays and validate each argument.
- For file paths, canonicalize paths and enforce allowed directories and filenames.

## Cryptography

- Use platform-approved cryptographic libraries and modern defaults.
- Do not create custom encryption, hashing, signing, random number generation, or key exchange algorithms.
- Use TLS for data in transit.
- Use authenticated encryption for sensitive data at rest where application-level encryption is needed.
- Use cryptographically secure random generators for tokens, secrets, reset links, API keys, and session identifiers.
- Keep secrets out of source code and load them from a secret manager or environment-specific secure configuration.
- Design for key rotation where practical.
- Verify TLS certificates and hostnames; do not disable verification to fix connectivity.
- Use established key management and purpose-separated keys where appropriate. Do not log cryptographic material.

## Security Configuration

- Fail closed when security-critical configuration is missing or invalid.
- Keep development, test, staging, and production configurations separate.
- Disable debug mode, stack traces, verbose errors, test endpoints, mock auth, sample credentials, and development-only tools in production.
- Set security headers where applicable, including Content Security Policy, X-Content-Type-Options, Referrer-Policy, frame protections, and appropriate cache controls.
- Restrict CORS to required trusted origins unless a resource is intentionally public and credential-free. Do not use wildcard CORS with credentials; CORS is not authorization.
- Enforce least-privilege permissions for services, databases, queues, storage buckets, and cloud resources.
- Use HSTS where appropriate for established HTTPS deployments; evaluate subdomain and preload implications before enabling them.
- Ensure development-only authentication mocks cannot be enabled in production. Do not relax CSP, TLS, or CORS to resolve implementation issues.

## Supply Chain Security

- Prefer well-maintained dependencies with clear provenance and active security support.
- Do not add packages for trivial functionality.
- Pin or lock dependencies according to the project’s package manager.
- Respect existing lockfiles.
- Check for known vulnerabilities when changing dependencies.
- Avoid install scripts, opaque binaries, abandoned packages, typosquatting risks, and packages with excessive privileges.
- Do not disable dependency scanning, SAST, secret scanning, or CI security checks.
- Verify that packages and versions exist in the intended registry. Check identity, publisher, provenance, support status, and security advisories before introducing dependencies. Do not invent package names or APIs.
- Avoid untrusted registries and unintended public fallback for private dependencies. Review relevant transitive changes and use existing SCA/SBOM tooling when available.
- Do not perform broad or forced upgrades without assessing compatibility and risk; report audit limitations and distinguish detected vulnerabilities from unverified concerns.

## Software and Data Integrity

- Verify authenticity and integrity of external data or artifacts using trusted signing keys, signatures, independently trusted digests, or protocol-specific verification as appropriate.
- Protect CI/CD workflows from untrusted pull request execution risks.
- Do not execute untrusted code, templates, serialized objects, plugins, or uploaded files.
- Use safe parsers and strict content-type checks.
- Treat third-party webhooks and callbacks as untrusted until verified.
- Pin or verify externally sourced build artifacts and actions using the project's trusted mechanisms. Protect CI secrets and restrict job permissions; never execute untrusted contributions in a privileged context.

## Error Handling and Exceptional Conditions

- Handle expected failure modes explicitly.
- Do not leak secrets, stack traces, SQL errors, internal paths, framework versions, tokens, or sensitive business logic in user-facing errors.
- Return safe, actionable external errors where appropriate and sanitized diagnostic internal logs; avoid revealing sensitive implementation details.
- Use consistent error responses.
- Ensure cleanup, rollback, rate limiting, and resource limits are applied during exceptional flows.
- Do not allow exception paths to bypass authorization, validation, logging, or transaction safety.
- Use bounded retries and timeouts; retry only when safe for the operation. Handle cancellation, partial writes, and dependency failures explicitly.
- Do not fall back to insecure authentication, authorization, or transport when a dependency fails.

## Logging, Monitoring, and Alerting

- Log security-relevant events, including authentication failures, authorization denials, privilege changes, sensitive data access, admin actions, token lifecycle events, validation failures, and suspicious activity.
- Do not log passwords, secrets, full tokens, private keys, sensitive personal data, payment data, or session identifiers.
- Use structured logs where possible.
- Include correlation/request IDs when available.
- Preserve enough context for investigation without exposing sensitive data.
- Ensure important security failures are observable and alertable.
- Prevent log injection by safely encoding untrusted values. Redact sensitive data before logging, tracing, or telemetry; avoid collecting it unnecessarily.
- Restrict access to logs and protect their integrity where required. Logging failures must not silently bypass mandatory security controls.

## Insecure Design Prevention

- Consider abuse cases, not just happy paths.
- Apply rate limiting, throttling, quotas, and replay protection to sensitive endpoints.
- Design workflows to prevent privilege escalation, business logic abuse, race conditions, and state confusion.
- Use explicit, server-enforced state transitions for sensitive multi-step flows such as checkout, onboarding, password reset, email change, role change, and account recovery; introduce state-machine abstractions when justified.
- Avoid trusting client-supplied prices, roles, ownership claims, feature flags, approval states, or workflow state.
- Prefer secure-by-default APIs and components.
- Identify trust boundaries, sensitive assets, actors, and abuse cases for material changes. Apply the threat-modeling skill when appropriate.
- Enforce security-sensitive state transitions atomically when required. Prefer proven controls over bespoke frameworks.

## Dependency and Tooling Rules

- Do not modify lockfiles manually except through the appropriate package manager.
- Do not disable security tooling, lint rules, type checks, tests, or CI gates without explicit justification.
- Do not suppress warnings unless the suppression is narrow, documented, and safe.
- Prefer project-standard libraries and patterns already present in the repository.
- Avoid introducing duplicate frameworks, validators, HTTP clients, crypto libraries, or logging systems.

## Secrets and Sensitive Data

Never create, print, commit, or expose real secrets.

Use placeholders such as:

- `EXAMPLE_API_KEY`
- `REPLACE_ME`
- `YOUR_SECRET_HERE`
- `example.com`
- `user@example.com`

When adding configuration examples, include safe sample values and document required environment variables.
- Never commit populated local environment files or real secrets in fixtures, examples, logs, or tests. Use project-approved secret storage and scope access to the minimum required.
- If a suspected exposed secret is found, report its location without reproducing its value; recommend revocation/rotation and approved remediation. Do not assume deleting it from the working tree removes it from history.

## Engineering Quality Requirements

- Preserve existing behavior unless the task requires a change. Prefer readable, strongly typed (where supported), testable code and existing project conventions.
- Preserve keyboard navigation, semantic markup, accessible labels, focus behavior, and usable authentication and error workflows. Do not introduce inaccessible security mechanisms when suitable accessible alternatives exist.
- Consider availability, resource consumption, operational failure modes, and privacy alongside security. Use synthetic or sanitized data in development and tests.
- Keep documentation current and explain non-obvious security decisions, including assumptions and residual risks.
