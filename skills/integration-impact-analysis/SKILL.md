---
name: integration-impact-analysis
description: General workflow for backend integration impact analysis across repositories and external systems. Use when assessing where a change belongs, which service owns the logic, what data is already available, the likely difficulty, and the remaining contract or payload unknowns before implementation.
---

# Integration Impact Analysis

Use this skill before or during backend integration work when ownership and difficulty are not yet clear.

## When to use

- New external integration
- New endpoint or callback from a partner system
- Cross-repo backend changes
- Contract mismatch analysis
- "Where should this be implemented?" questions

## Default approach

1. Identify the real owner path:
   - facade or gateway
   - business owner service
   - outbound client
   - persistence owner
2. Map the end-to-end flow:
   - incoming trigger
   - service boundary
   - data assembly
   - external request
   - response handling
   - persistence and status updates
3. Classify what is already known:
   - verified from repo
   - verified from API contract or examples
   - assumed but not yet proved
4. Judge difficulty from the real integration surface, not from file count.
5. If enough is clear, move directly to implementation in the owner slice.

## Questions to answer

- Which repository owns the business logic versus the exposure layer?
- Is the current repo only a facade?
- Which fields already exist internally?
- Which fields still lack a trusted source?
- Is the missing piece code, configuration, data mapping, or external contract clarification?
- What is the first thin slice that can be implemented safely?

## Deliverable format

Provide:

- architecture of the flow
- exact attachment point
- direct backend responsibilities
- external or shared dependencies
- difficulty judgment
- 2 or 3 real open questions, if any

## Guardrails

- Do not claim field provenance unless repo code, API contract, examples, or data checks support it.
- Keep "implemented" separate from "blocked by missing contract proof".
- Prefer the smallest end-to-end slice that creates tangible progress.
