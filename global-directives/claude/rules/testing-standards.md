---
paths:
  - "**/*test*"
  - "**/*spec*"
  - "**/__tests__/**"
---

# Tests must prove intent

Tests must verify business behavior, correctness guarantees, and regression prevention.

Avoid tests that only verify existence, non-null output, mocked calls, or constant returns. Prefer focused regression tests.