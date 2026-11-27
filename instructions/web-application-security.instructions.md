---
name: Web Application Security Policy
---

# Web Application Security Policy — v2.0

Apply these domain-specific requirements when the relevant application surface is present. Use together with [Core Secure Coding Policy](./secure-coding.instructions.md). This file intentionally has no blanket `applyTo` pattern; the secure coding agent explicitly reads it. Do not infer authorization from client input, file extensions, or framework conventions alone.

## Access Control

- Enforce authorization on the server side for every protected action.
- Do not rely on hidden UI elements, disabled buttons, client-side route guards, or frontend checks as authorization controls.
- Use deny-by-default authorization.
- Check object ownership and tenant boundaries before reading, updating, deleting, exporting, or sharing data.
- Prevent IDOR by validating that the authenticated principal is authorized to access the requested resource.
- Keep privileged operations isolated and auditable.
- Enforce function-, object-, and property-level authorization, including list, search, aggregation, bulk, export, and download operations. Explicitly restrict readable and writable fields.
- Derive identity, roles, and tenant context from trusted authentication and authorization mechanisms, not client claims. Enforce tenant boundaries in caches, storage, queues, and background jobs as well as databases.
- Reevaluate authorization for deferred operations as appropriate; enforce permission changes and revocation consistently.

## Authentication and Session Security

- Use established framework authentication mechanisms where available.
- Do not implement custom password hashing, token signing, or session management unless specifically required.
- Store passwords only with strong adaptive hashing such as Argon2id, bcrypt, or scrypt.
- Set secure cookie attributes where applicable: `HttpOnly`, `Secure`, `SameSite=Lax` or stricter.
- Regenerate sessions after login and privilege changes.
- Do not expose session or access tokens in URLs, logs, analytics, or error messages. Avoid persistent browser storage of sensitive tokens; use architecture-appropriate mitigations. Short-lived recovery/verification links require separate purpose-bound safeguards.
- Apply MFA-aware and account-lockout-aware design where relevant.
- Set appropriate idle and absolute session expiration; invalidate sessions and revoke credentials where applicable. Require reauthentication for sensitive account and privilege changes.
- Prevent account enumeration and credential stuffing using consistent responses, rate limits, and appropriate MFA-aware controls; avoid lockout designs that enable abuse.
- Use short-lived, single-use, purpose-bound reset and verification tokens; prevent reuse, leakage, and unsafe redirects. Avoid logging or caching token-bearing URLs; apply restrictive referrer policy where needed.
- Validate token signatures, allowed algorithms, issuer, audience, expiration, and intended purpose. Use established OAuth 2.0/OpenID Connect libraries; apply PKCE, state, and nonce as required by the flow. Avoid deprecated flows and validate redirect URIs.

## Frontend Security Requirements

- Do not store sensitive long-lived tokens in local storage.
- Avoid unsafe DOM manipulation.
- Encode untrusted content for its rendering context; sanitize intentionally supported HTML with a maintained sanitizer rather than treating all user text as HTML.
- Prefer framework-native escaping.
- Use CSRF protection for cookie-authenticated state-changing requests.
- Avoid exposing secrets in bundled code, environment variables intended for the server, source maps, logs, or telemetry.
- Validate client-side for usability, but always enforce validation server-side.
- Apply accessibility and security together; do not remove labels, focus behavior, or safe browser defaults without reason.
- Validate untrusted navigation and resource URLs; reject unsafe schemes and redirects. Use maintained HTML sanitizers for intentionally supported rich content.
- Verify exact origins, sources, and message schemas for cross-origin messaging; restrict target origins when sending sensitive messages.
- Avoid weakening CSP; use appropriate cache controls for sensitive authenticated responses and prevent leakage through browser caches, telemetry, and client state.
- For WebSockets and other persistent connections, enforce authentication, origin checks where relevant, message-level authorization and validation, and resource limits.

## Backend and API Security Requirements

- Authenticate and authorize every protected route, resolver, controller, job, and handler.
- Validate request bodies, query parameters, route parameters, headers, uploaded files, and webhook payloads.
- Use schema validation where practical.
- Apply rate limits to authentication, password reset, invitation, search, export, upload, and expensive endpoints.
- Enforce pagination and maximum page sizes.
- Use idempotency keys for payment, order, and other sensitive write operations where appropriate.
- Avoid mass assignment by explicitly mapping allowed fields.
- Use transactions for multi-step state changes.
- Protect against SSRF when fetching URLs supplied by users or third parties.
- Restrict outbound fetches to necessary schemes and destinations; validate resolved addresses and redirect targets, prevent internal/metadata access unless explicitly required, and use network-level restrictions where available. Consider DNS rebinding and resolution/connect consistency.
- Enforce body size, content types, timeouts, concurrency, and resource limits. Bound external response sizes and retries. For GraphQL, apply appropriate depth/complexity and resolver authorization limits.
- Explicitly map response fields instead of exposing whole persistence objects. Validate redirect targets against allowed destinations or safe local paths.
- Scope idempotency keys to identity/tenant and operation; reject conflicting reuse and ensure atomic handling. Use constraints, transactions, or locks to avoid race conditions and duplicate effects.
- Verify webhook authenticity over the required payload representation, check timestamps and replay protection where supported, and make handlers idempotent where appropriate.

## Database and Persistence Requirements

- Use migrations for schema changes.
- Preserve data integrity with constraints, indexes, foreign keys, and uniqueness checks where appropriate.
- Avoid storing sensitive data unless required.
- Minimize retained personal data.
- Use parameterized queries or safe ORM methods.
- Use opaque identifiers where appropriate to reduce enumeration, but enforce object-level authorization regardless of identifier format. Unpredictable identifiers are not authorization.
- Consider row-level tenant isolation for multi-tenant systems.
- Apply data classification, retention, deletion, and minimization requirements across databases, caches, queues, exports, logs, and temporary files.
- Scope caches to authorization-relevant identity/tenant/context; prevent cross-user leakage and invalidate or revalidate sensitive cached data when permissions or state change.
- Use synthetic or sanitized data for development and tests. Restrict database permissions and preserve compatibility and rollback safety of migrations.

## File Upload and Download Requirements

- Treat uploaded files as untrusted.
- Validate file type, size, extension, content type, and magic bytes where practical.
- Store uploads outside executable paths.
- Generate server-side filenames.
- Strip or control metadata when needed.
- Scan files if the project has malware scanning support.
- Enforce authorization on downloads.
- Use safe content disposition and content type headers.
- Treat original filenames and metadata as untrusted; prevent traversal, unsafe symbolic-link resolution, and unauthorized access. Use safe path handling and enforce containment.
- Enforce extracted/decompressed size, entry count, and processing limits for archives; prevent unsafe extraction paths. Limit parsing and transformation resources.
- Prevent uploaded active content (such as HTML/SVG) from executing in the trusted application origin; isolate or serve as safe downloads where appropriate.
- Use private storage and narrowly scoped, expiring access URLs when appropriate. Do not expose unverified or partially processed files; protect temporary files and cleanup paths.
