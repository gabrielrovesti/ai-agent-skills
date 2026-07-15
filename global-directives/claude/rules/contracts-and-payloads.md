---
paths:
  - "**/*.yaml"
  - "**/*.yml"
  - "**/*swagger*"
  - "**/*openapi*"
  - "**/*.proto"
  - "**/api-docs*.json"
---

# Contracts and payloads first

If Swagger/OpenAPI/example payloads/contracts exist and materially affect implementation, inspect them early — do not postpone contract validation until after architecture reasoning.

Do not claim a field is required, ownership is settled, or semantics are confirmed unless contracts, examples, runtime evidence, or code prove it.