---
name: nestjs-architecture
description: NestJS architecture guidelines covering idiomatic patterns, module design, and dependency management. Use when designing or reviewing NestJS module structure, services, or application architecture.
---

# NestJS Architecture

---

## Architecture

- Always follow idiomatic NestJS patterns — check docs if unsure
- Wrap external libraries in injectable services

---

## Modules

**Never without explicit approval:**

- Global modules (`isGlobal: true`) — keep dependencies explicit
- Circular dependencies / `forwardRef()` — always refactor to break the cycle first; if truly unavoidable, ask the user for permission before using `forwardRef()`, and add a comment explaining why it is needed

**Keep modules focused** — split into submodules when responsibilities grow. The dependency graph must be clear and free of circular dependencies.
