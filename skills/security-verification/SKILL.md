---
name: security-verification
description: Verify completed web application changes with relevant project checks, evidence-based security control status, final diff review, and accurate residual-risk reporting.
---

# Security Verification

## Procedure

1. Review the final diff and worktree status. Confirm the requested scope, no unintended modifications, and relevant documentation updates. Identify applicable security controls from the task and policies.
2. Confirm authentication, authorization, input validation, output encoding, parameterization, secret handling, safe failure paths, logging, configuration, dependencies, and security tests where relevant. Record concrete evidence rather than assumptions.
3. Identify existing validation commands from project documentation, manifests, and CI. Inspect unfamiliar scripts. Run the applicable formatting, lint, type checks, unit tests, integration tests, security scans, dependency audits, and build in a safe environment if available. Avoid network or privileged operations without authorization.
4. Do not fabricate results. Record the actual command, exit/result, relevant failure summary, and skipped/unavailable checks with reasons. Distinguish pre-existing failures only with evidence; do not change tests or security gates merely to pass.
5. Classify applicable controls as VERIFIED (evidence supports the stated property within scope), FAILED (evidence shows failure), NOT VERIFIED (insufficient evidence), or NOT APPLICABLE (reason it is outside scope). If a critical control is failed or unverified, do not describe the change as securely complete; report required follow-up.
6. Provide concise completion reporting, proportional to risk. Passing automation is not certification or proof of absence of vulnerabilities.

## Verification report template

**Change / scope:**

| Control | Status | Evidence or reason |
| --- | --- | --- |

| Command/check | Result (passed/failed/skipped/unavailable) | Evidence or reason |
| --- | --- | --- |

**Residual risks / assumptions / blockers:**

**Documentation updated:**

For trivial changes, summarize only the relevant checks and limitations. Never claim a test, scan, or build was run unless actually executed.

Apply the agent’s mandatory safeguards and applicable security policies. Use the [core policy](../../instructions/secure-coding.instructions.md) and [web application policy](../../instructions/web-application-security.instructions.md) from this installation when relevant; read them if not already loaded. Stay within the authorized task. Do not claim unavailable guidance or checks were used.
