# Testing And Verification

Use this reference when the persistence change needs proof, not just code plausibility.

## Verification order

1. targeted repository or service test for the changed behavior
2. `@DataJpaTest` or equivalent slice if repository behavior changed
3. containerized integration test when the issue depends on the real database
4. broader module test only when risk justifies it

## `@DataJpaTest`

Use for:

- repository query behavior
- mapping and constraint validation
- projection correctness
- custom repository logic

## When Testcontainers matters

Prefer a real database container when the behavior depends on:

- database-specific SQL
- indexing behavior
- sequences or identity strategy
- JSON, array, or full-text features
- transaction or locking semantics

## What to assert

- returned rows and ordering
- pagination boundaries
- projection field mapping
- absence of duplicate rows after fetch joins
- bulk operation side effects
- migration compatibility where relevant

## Query-regression proof

If the problem is performance-sensitive, prove at least one of:

- query count reduced
- projection replaced full entity loading
- pagination added to an unbounded path
- repository method no longer triggers lazy traversal outside the intended transaction

## Reporting gaps

If verification cannot run, say exactly why:

- missing database
- container access unavailable
- migration not applied
- test fixture incomplete
