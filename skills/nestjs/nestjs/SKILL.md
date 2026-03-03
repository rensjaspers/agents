---
name: nestjs
description: NestJS development guidelines covering CLI generation, architecture patterns, and module design. Use when writing or modifying NestJS backend code, modules, or services.
---

# NestJS

---

## Code Generation

Use the NestJS CLI to generate modules, services, controllers, and resources.

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
