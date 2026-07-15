---
paths:
  - "**/Dockerfile*"
  - "**/docker-compose*"
  - "**/.devcontainer/**"
  - "**/Makefile"
---

# Containers preferred, not mandatory

Prefer existing container workflows (Dockerfile, docker-compose, devcontainer, Makefile targets) when present. Do not install host packages unless explicitly requested. Do not create container infrastructure unless the task justifies it.