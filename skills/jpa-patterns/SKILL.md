---
name: jpa-patterns
description: "Designs and diagnoses JPA/Hibernate persistence in Spring Boot services: entity boundaries, repository patterns, projections, relationships, transaction scope, N+1 and lazy-loading issues, persistence-side performance tuning, and dependency or migration checks that materially affect data access tests or runtime behavior. Use when the task is specifically about persistence design, repository/query implementation, Hibernate behavior, or a backend issue whose cause may sit in the JPA layer."
---

# JPA Persistence Decisions

Use this skill when the task is primarily about how a Spring Boot service models, queries, writes, or verifies data through JPA/Hibernate.

## Critical rules

- Do not create repositories for every entity by default. Confirm the real aggregate or ownership boundary first.
- Do not treat a persistence issue as code-only before checking query shape, fetch plan, transaction scope, and relevant schema or migration state.
- Do not use entities for read-heavy responses when a projection or query model is a better fit.
- Do not use `save()` blindly when the difference between insert, update, merge, flush timing, or detached state matters.
- Do not add or keep `EAGER` fetching as a generic fix for `LazyInitializationException`.
- Do not discuss persistence changes without checking test and dependency impact when the change touches migrations, database drivers, or containerized integration tests.

## Workflow

### Step 1: Classify the real persistence problem

Identify which of these is actually being asked:

1. entity or aggregate boundary
2. repository or query design
3. relationship mapping
4. transaction or write-path behavior
5. read-path performance
6. migration, dependency, or test impact

### Step 2: Inspect the real flow before proposing changes

Read the smallest complete slice that owns the behavior:

- entity and repository
- service transaction boundary
- mapper, DTO, or projection
- migration files when schema shape matters
- nearby tests
- build file when dependency or version changes are involved

If the issue is user-visible, reconstruct the runtime path from controller or scheduler to repository and back.

### Step 3: Load only the references that match the problem

Use progressive disclosure:

| Problem | Read |
|---|---|
| aggregate ownership, repository boundaries, entity vs ID reference | `references/repository-boundaries.md` |
| derived query vs `@Query` vs projection vs custom repository vs query service | `references/query-and-projection-patterns.md` |
| relationship mapping and fetch strategy | `references/relationships.md` |
| N+1, lazy loading, pagination, batching, indexing, read-only tuning | `references/performance-diagnosis.md` |
| Flyway/Liquibase, driver or starter changes, Boot/Testcontainers drift | `references/migrations-and-dependencies.md` |
| `@DataJpaTest`, Testcontainers, query regression proof, verification order | `references/testing-and-verification.md` |

### Step 4: Choose the narrowest correct pattern

Prefer:

- derived query for trivial lookups
- `@Query` for readable joins and multi-filter queries
- DTO or record projections for read-only responses
- custom repository or query service for dynamic criteria, bulk operations, or read models that do not fit entity loading
- ID references instead of entity navigation when module coupling is not justified

### Step 5: Validate the downstream impact

Before finalizing the solution, check:

- transaction scope
- lazy-loading behavior
- row growth and pagination needs
- batch or flush implications
- schema migration impact
- test coverage on the changed read or write path

If dependency or version work is involved, confirm the change in the build file and name the exact tests that should prove the migration.

## Red flags

- controller or mapper traverses lazy collections outside a clear transaction
- `@ManyToMany` used where a join entity carries real meaning
- unbounded list queries on endpoints or schedulers
- entity returned when the caller needs only a summary view
- write logic hidden inside repository default methods or custom query side effects
- production schema change implied but no migration file updated
- persistence change proposed without checking integration tests or container setup

## Output format

When proposing or implementing a persistence change, return:

```markdown
## Persistence judgment
- Problem type:
- Recommended pattern:
- Why this is the narrowest correct choice:

## Files to inspect or change
- `path/to/file`

## Verification
- Targeted tests:
- Query or runtime behavior to prove:

## Risks
- ...
```

## When not to use this skill

- generic SQL administration with no Spring Data JPA involvement
- broad architecture design not centered on persistence
- frontend-only problems
- version-upgrade work that does not materially touch persistence, migrations, or persistence tests
