---
name: springboot-ticket-diagnosis
description: Spring Boot workflow for diagnosing support tickets and user-visible backend behavior. Use when tracing endpoint or scheduler behavior, separating immediate effects from delayed jobs, checking profiles, filters, DB-driven gates, and proving whether a problem is code, data, configuration, contract, or expected business behavior.
---

# Spring Boot Ticket Diagnosis

Use this skill for ticket-style backend diagnosis in Spring Boot services when the goal is to reach a verified cause judgment, not just to explain the code.

## When to use

- support-ticket diagnosis on a Spring Boot service
- user-visible behavior that may depend on backend logic
- endpoint or scheduler behavior reconstruction
- disputes about whether something is a bug, expected behavior, or data/config drift
- backend/frontend cross-checks where backend truth must be established first

## When not to use

- broad feature implementation with no diagnostic angle
- pure SQL-only investigations where Java behavior is already irrelevant
- generic architecture design

## Default workflow

1. Define the real question in operational terms.
   - what did the user expect
   - what actually happened
   - whether the ask is diagnosis, confirmation, or fix
2. Identify the real entrypoint.
   - controller endpoint
   - scheduler
   - consumer/listener
   - batch trigger
3. Reconstruct the runtime path in order.
   - request or trigger
   - guards and validation
   - service decisions
   - repository and external-client calls
   - response mapping
   - async or scheduled follow-up work
4. Check non-code gates before blaming Java logic.
   - DB rows
   - catalog/config flags
   - profiles
   - environment-dependent filters
   - contract or payload constraints
5. Close with a cause judgment:
   - expected business behavior
   - data/config problem
   - contract mismatch
   - possible code defect

## Diagnosis checklist

- Which class owns the user-visible decision?
- Is the behavior driven by code, query results, configuration, or external response?
- Is there a fallback path?
- Is there a profile or role-based filter?
- Is the behavior immediate, delayed, or both?
- Does frontend behavior depend on a backend field whose semantics must be checked?
- Is the issue date-sensitive or availability-sensitive?

## Proof expectations

- cite the entrypoint class and the service owner
- cite the key repository, query, or client call involved
- if the result depends on data, show the needed DB/config proof
- if the result depends on timing, separate immediate effects from scheduled effects

## Guardrails

- Do not generalize date-sensitive behavior from a partial sample.
- Do not claim runtime truth from static code alone when logs, payloads, or DB checks are available.
- Do not stop at backend code if frontend wiring can materially alter visible behavior.
- Do not overstate certainty when the real blocker is missing data or missing production proof.

## Output shape

Prefer:

- verified facts
- diagnosis
- practical next step

If the user asks for a yes/no conclusion, end with a direct confirm or deny verdict.
