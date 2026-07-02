---
name: java-backend-workflow
description: General workflow for Java backend tasks in Spring Boot or similar services. Use when diagnosing behavior, implementing a change, tracing a runtime flow, or reviewing a backend fix where correctness, minimal diffs, callers, SQL touchpoints, and targeted verification matter more than broad redesign.
---

# Java Backend Workflow

Use this skill for the common backend path: understand the real flow, make the smallest correct change, and verify the intended behavior.

## When to use

- Java backend bug fixes
- Small or medium backend features
- Runtime flow explanation from endpoint to DB or external service
- Refactoring constrained to one feature slice
- Code review of a backend change

## Default approach

1. Define the target before editing:
   - goal
   - acceptance criteria
   - affected files or layers
   - likely verification command
   - whether live docs or current version info are needed
2. Read before writing:
   - entrypoint controller or scheduler
   - direct service path
   - DTOs, repositories, clients, mappers
   - immediate callers
   - nearby tests
   - shared utilities already used in that slice
3. Reconstruct the runtime flow in order:
   - inbound request or trigger
   - validation and guards
   - business branching
   - persistence and external calls
   - response mapping
   - error handling
4. Change only the narrow slice required by the acceptance criteria.
5. Verify with the smallest useful proof first, then broader checks if needed.

## Working rules

- Prefer existing patterns over cleaner but inconsistent alternatives.
- Do not create a new abstraction unless the same logic is clearly repeated.
- Check whether the real issue is data, configuration, or contract drift before changing Java code.
- If behavior depends on SQL results, enums, catalog tables, or profile/config flags, verify those inputs explicitly.
- When two patterns conflict, follow the one closest to the edited code.
- Explain facts and assumptions separately when reporting the result.

## Code-reading checklist

- Which class is the real owner of the decision?
- Is the behavior hardcoded, enum-driven, query-driven, or config-driven?
- Is there a fallback path?
- Is there async or scheduled behavior separate from the immediate call?
- Is a missing result treated as normal, filtered, or exceptional?
- Are there profile-specific filters or environment gates?

## Implementation guardrails

- Keep diffs small and reversible.
- Preserve existing logging and exception style unless the task is specifically about them.
- Prefer explicit names over compact clever code.
- Add comments only when the code path is hard to infer.
- Avoid speculative performance work unless the task is about performance.

## Verification order

1. Focused test for the changed behavior
2. Narrow build or module test command
3. Broader verification only when the risk justifies it

If verification cannot run, state the exact gap.

## Final answer contract

Include:

- what changed
- why this is the correct slice
- what was verified
- remaining uncertainty, if any
