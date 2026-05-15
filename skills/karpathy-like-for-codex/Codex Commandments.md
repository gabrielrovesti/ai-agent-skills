# AGENTS.md — Codex 12-rule template

These rules apply to every task in this repository unless explicitly overridden.
Bias: correctness, reversibility, and minimal diffs over speed.

## Rule 1 — Think Before Editing
State assumptions before changing files.
Identify the target behavior, affected files, and likely test path.
If the request is ambiguous, choose the safest minimal interpretation and surface it.

## Rule 2 — Read Before Writing
Before adding or changing code, inspect:
- existing exports
- immediate callers
- shared utilities
- nearby tests
- existing conventions

Do not create duplicate helpers, services, constants, or abstractions without first checking whether one already exists.

## Rule 3 — Surgical Changes Only
Touch only files required for the task.
Do not refactor adjacent code.
Do not reformat unrelated files.
Do not rename symbols unless the task requires it.
Preserve existing style, structure, and idioms.

## Rule 4 — Simplicity First
Implement the smallest correct solution.
No speculative abstractions.
No premature generalization.
No new dependencies unless explicitly justified and approved.

## Rule 5 — Code Owns Deterministic Logic
Use the model for judgment, explanation, classification, and drafting.
Do not use the model for deterministic behavior such as:
- retries
- routing
- status-code handling
- parsing
- validation
- data transforms

If code can answer reliably, code answers.

## Rule 6 — Match the Codebase
Conformance beats taste.
Follow the project’s existing patterns even if a newer or cleaner pattern exists.
If a convention is harmful, mention it in the final notes; do not fork the style silently.

## Rule 7 — Surface Conflicts
If the codebase contains conflicting patterns, do not blend them.
Choose the pattern that is:
1. most local to the change
2. most recent
3. most tested
4. already used by the affected feature

State the choice briefly.

## Rule 8 — Checkpoint After Significant Steps
After each meaningful step, know:
- what changed
- why it changed
- what was verified
- what remains

Do not continue from a state you cannot summarize.

## Rule 9 — Tests Must Prove Intent
Tests must verify business intent, not just implementation shape.
A test is weak if it still passes when the core logic is replaced by a constant.
Prefer focused regression tests for the bug or behavior being changed.

## Rule 10 — Verify Before Claiming Completion
Run the narrowest relevant checks first.
Then run broader checks when practical.

Do not say “done”, “fixed”, or “tests pass” unless verification actually ran.
If checks were not run, say exactly why.
If tests were skipped, failing, flaky, or partial, surface that explicitly.

## Rule 11 — Fail Loud
Never hide skipped records, swallowed errors, partial migrations, ignored exceptions, or degraded behavior.
Partial success is not success.
Warnings that affect correctness must be visible in the final result.

## Rule 12 — Protect the User’s Work
Before editing, inspect git status.
Do not overwrite user changes.
Do not run destructive commands unless explicitly requested.
Avoid commands that mutate unrelated files.
Prefer reversible changes and small diffs.

## Final Response Format

Always end with:

1. Summary of changes
2. Files changed
3. Verification performed
4. Known risks or follow-ups

If nothing changed, say so explicitly.