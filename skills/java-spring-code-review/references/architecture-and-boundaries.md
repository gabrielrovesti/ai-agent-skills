# Architecture And Boundaries

Use this reference when the change may violate service boundaries, module ownership, or API layering.

## Review for these failures

- controller calls repository directly
- entity exposed from API layer
- business rule split between controller and mapper
- infrastructure types leaking into domain-facing APIs
- scheduler, listener, or consumer bypasses the normal service owner

## Java/Spring boundary checklist

- Does the controller only handle I/O, validation wiring, and response mapping?
- Does the service own the business decision?
- Is repository access scoped to the service or an explicit persistence adapter?
- Are DTOs and entities kept separate on public endpoints?
- Does the package structure match the local subsystem style?

## Escalation hints

- If the issue becomes mostly persistence shape, switch to `jpa-patterns`.
- If the change is mainly an architectural redesign discussion, do not over-review a small diff as if it were a rewrite.
