# Security Review Checks

Use this reference when the diff touches endpoints, auth, validation, secrets, outbound calls, or error handling.

## High-signal checks

- privileged action with no `@PreAuthorize` or equivalent guard
- user input reaching query construction unsafely
- missing `@Valid` or weak field constraints on request DTOs
- password, token, cookie, or PII logged
- raw exception details returned to clients
- user-controlled URL or host used in outbound HTTP calls without allowlist validation

## Review questions

- Who is allowed to call this path?
- What input is trusted here, and why?
- Can the response leak internals?
- Are secrets sourced from config safely?
- Are security-relevant failures logged without exposing sensitive values?

## Common Spring-specific smells

- `permitAll()` wider than intended
- endpoint-level guard present but service path callable elsewhere without protection
- native query or string-built query with user input
- auth flow changes without corresponding tests
