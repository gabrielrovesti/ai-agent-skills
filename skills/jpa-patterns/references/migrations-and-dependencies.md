# Migrations And Dependencies

Use this reference when a persistence change also affects build configuration, migrations, database compatibility, or integration-test infrastructure.

## Start with inspection

Read before proposing changes:

- `pom.xml` or `build.gradle`
- Flyway or Liquibase files
- test dependencies for database or container support
- driver and dialect configuration if relevant

## Persistence-side migration checklist

- Is schema shape changing?
- Is a migration file required?
- Is the database driver or dialect changing?
- Do integration tests depend on Testcontainers or embedded databases?
- Does a framework upgrade move or deprecate persistence-test annotations or APIs?

## Rules

- Never rely on Hibernate auto-DDL in production migrations.
- Keep schema migrations explicit, ordered, and reviewable.
- If the change affects DDL or query compatibility, name the database flavor that must validate it.
- If dependency changes touch integration tests, state the exact tests that should run after the change.

## Flyway and Liquibase

Prefer additive migrations first:

- add columns before backfilling
- backfill before enforcing `NOT NULL`
- avoid destructive drops without a rollback or cleanup plan

## Dependency touchpoints worth checking

- JPA and Hibernate version bumps
- database driver changes
- migration tool versions
- Testcontainers artifact or package changes
- logging or SQL-inspection libraries used only in tests

## Typical follow-up proof

- schema migration applied successfully
- application starts with the new schema
- repository tests still pass
- containerized integration tests still boot against the expected database
