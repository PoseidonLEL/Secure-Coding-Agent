---
name: secure-implementation
description: Implement or refactor web application features and fixes using existing project conventions, secure defaults, minimal changes, and applicable security controls.
---

# Secure Implementation

## Procedure

1. Confirm task scope, repository context, and applicable policies. Use repository discovery when context is missing and threat modeling for material security changes.
2. Define intended behavior, trust boundaries, acceptance criteria, security invariants, and failure behavior. Reuse established authentication, authorization, validation, persistence, logging, and error-handling mechanisms.
3. Implement the smallest safe change. Enforce controls at the authoritative boundary, including denied and exceptional paths. Protect sensitive data and use framework-supported safe APIs.
4. Verify introduced libraries and APIs against project versions and intended registries. Avoid new dependencies unless necessary; do not bypass tooling or use insecure defaults to unblock work.
5. Add or update meaningful functional and security tests. Use security-testing for security-sensitive changes. Ensure migrations, configuration, and documentation are updated when required.
6. Review the diff for unexpected files, exposed secrets, permission changes, unsafe failure paths, and regressions. Preserve user changes. Use security-code-review for material security changes and security-verification before completion.

## Expected output

Implementation and relevant tests/documentation, with a concise explanation of security decisions and unresolved assumptions. For example, a resource retrieval change should scope the lookup to authorized identity and tenant context and test a denied cross-account request. Do not implement unrelated findings unless authorized.

Apply the agent’s mandatory safeguards and applicable security policies. Use the [core policy](../../instructions/secure-coding.instructions.md) and [web application policy](../../instructions/web-application-security.instructions.md) from this installation when relevant; read them if not already loaded. Stay within the authorized task. Do not claim unavailable guidance or checks were used.
