---
name: java-spring-code-review
description: "Reviews Java and Spring Boot code changes with a findings-first workflow focused on behavioral regressions, contract drift, transaction and data-access risks, scheduler or async timing issues, security flaws, and performance problems. Use when the user asks to review code, inspect a PR or diff, check whether a backend change is safe, or evaluate a Java/Spring implementation before merge."
---

# Java Spring Code Review

Use this skill for review work on Java and Spring Boot code when the goal is to produce concrete findings grounded in the actual diff and nearby code, not generic style advice.

## Critical rules

- Never review without code context. Start from the changed files, diff, or named module.
- Never lead with summary. Findings come first, ordered by severity.
- Always cite file paths and line numbers for findings.
- Prefer correctness and regression risk over cleanliness or taste.
- Distinguish verified findings from open questions or missing-proof areas.
- Do not recommend broad redesigns unless the current change clearly creates a real defect or maintenance trap.

## Review workflow

### Step 1: Confirm the review surface

Collect the smallest useful scope:

1. changed files or diff
2. build file when versions or dependencies may matter
3. direct callers, repository methods, DTOs, mappers, config, or tests touched by the change
4. whether the user wants findings only, or also fix direction

### Step 2: Reconstruct intent before judging

Read enough nearby code to answer:

- what behavior is changing
- what was previously guaranteed
- what inputs, data, or timing assumptions the code relies on
- what verification should prove the change safe

### Step 3: Load only the references that match the change

Use progressive disclosure:

| If the change touches | Read |
|---|---|
| controller-service-repository boundaries, module leaks, entity exposure | `references/architecture-and-boundaries.md` |
| auth, authz, validation, secrets, unsafe error exposure, SSRF, SQL injection | `references/security-review-checks.md` |
| N+1, pagination, caching, async work, connection pooling, resource leaks | `references/performance-review-checks.md` |
| dependency bumps, starter changes, test annotation drift, framework migration leftovers | `references/version-and-migration-signals.md` |

Escalate when needed:

- Use `jpa-patterns` for deep persistence design or Hibernate-specific diagnosis.
- Use `springboot-ticket-diagnosis` when the real question is user-visible runtime behavior rather than code quality alone.

### Step 4: Run review passes in this order

#### Pass A: Behavioral correctness

- wrong branch, condition, null handling, or exception behavior
- missing caller impact
- mismatch between changed code and surrounding assumptions

#### Pass B: Contract and data impact

- DTO/entity/API drift
- repository or query changes with hidden behavior changes
- transaction boundary mistakes
- scheduler or async timing hazards

#### Pass C: Security and operational safety

- authorization gaps
- input validation gaps
- secrets or sensitive logging
- unsafe outbound calls or error exposure

#### Pass D: Performance and maintainability

- N+1 or unbounded queries
- missing pagination or projection
- unnecessary blocking, over-fetching, or connection misuse
- patterns that will likely regress under normal growth

## What counts as a finding

Report a finding only when you can explain:

- where it is
- why it is risky or wrong
- what behavior can break, leak, or regress

Avoid low-value comments such as naming preferences, stylistic rewrites, or hypothetical abstractions unless they create a concrete defect risk.

## Output format

Use this shape:

```markdown
## Critical
- **[Category]** Short finding summary.
  - **File**: `path/to/File.java:123`
  - **Impact**: concrete failure, leak, or regression risk
  - **Why**: short code-grounded explanation

## High
- ...

## Medium
- ...

## Low
- ...
```

After findings, include only if needed:

- `Open questions`
- `Residual risks`
- brief `Change summary`

If there are no findings, say that explicitly and mention any blind spots such as unreviewed modules, missing tests, or unavailable runtime context.

## When not to use this skill

- frontend-only reviews
- generic clean-code coaching with no files or diff
- pure architecture brainstorming with no concrete code surface
