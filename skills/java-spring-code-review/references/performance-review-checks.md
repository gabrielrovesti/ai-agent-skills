# Performance Review Checks

Use this reference when the diff changes queries, loops over entities, async work, caching, or external calls.

## High-signal checks

- entity traversal in loops causing N+1
- unbounded list query added to endpoint, job, or export path
- entity returned where projection or summary would be enough
- missing pagination on potentially growing result sets
- async or scheduled work with no timeout, back-pressure, or ownership clarity
- new HTTP or DB client created per request

## Review questions

- Is the query shape appropriate for the caller?
- Does the code fetch more than it needs?
- Can row count grow beyond today's examples?
- Is this path request-time, scheduled, or batch?
- Is there a real reason to cache, parallelize, or batch here?

## Escalation hints

- For JPA-specific fixes, hand off to `jpa-patterns`.
- Do not recommend concurrency or cache changes without code evidence that they solve a current problem.
