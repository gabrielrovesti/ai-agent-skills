---
name: oracle-sql-diagnosis
description: Oracle SQL workflow for backend diagnosis, support analysis, and data validation. Use when the answer depends on proving behavior through queries, checking day-by-day availability or state, validating catalog/config rows, or separating code defects from data defects.
---

# Oracle SQL Diagnosis

Use this skill when SQL evidence is central to the answer. The goal is to prove or disprove behavior with focused queries, not to speculate from code alone.

## When to use

- Support or ticket diagnosis
- Data mismatch between UI, API, and expected business state
- Availability, booking, assignment, or history investigations
- Catalog, lookup, enablement, or profile checks
- Verification after a backend fix

## Default approach

1. Clarify the exact business question in one sentence.
2. Identify the minimal entities involved:
   - user
   - object or document
   - status
   - date range
   - configuration tables
3. Start with proof queries, not broad dumps.
4. Build from facts:
   - existence
   - current state
   - historical state
   - joins that explain visibility or eligibility
5. Only after SQL evidence, decide whether the issue is:
   - expected business behavior
   - missing or wrong data
   - configuration/catalog drift
   - possible code defect

## Query rules

- Prefer small, targeted selects first.
- Filter by concrete identifiers and dates whenever possible.
- For date-sensitive behavior, inspect the relevant days explicitly. Do not generalize from one day to a wider period without proof.
- When counts matter, pair aggregate queries with one inspection query so totals are explainable.
- When visibility depends on multiple joins, validate each join edge separately before producing the final combined query.
- If a row is expected but missing, check upstream lookup tables before blaming application logic.

## Investigation checklist

- Is the entity present?
- Is the status the one the code expects?
- Is there a lookup or catalog row gating the behavior?
- Is there a date-validity window?
- Is there a group, role, profile, or property relationship involved?
- Is the issue about current state or propagation delay?
- Is there a reserved or exceptional flag changing normal behavior?

## Output style

Separate:

- facts proved by query
- inference from those facts
- still-unverified assumptions

For support use cases, end with a short operational diagnosis that someone else can act on.

## Verification habits

- Keep reusable generic verification queries when they are genuinely useful.
- If a business rule is the real cause, explain it in plain language first and only then attach the SQL proof.
- Do not claim a systemic rule from a partial sample.
