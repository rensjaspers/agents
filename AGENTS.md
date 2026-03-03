# Agent Guidelines

This repository uses the following structure:

```
rules/       # Coding rules (.mdc format)
skills/      # Framework and tool skills
agents/      # Agent definitions
templates/   # Project-specific templates
```

## Rules

- [code-style](./rules/code-style.mdc) — Function size, control flow, DRY, comments
- [file-moves](./rules/file-moves.mdc) — Always use `mv` to move files

## Skills

Located in `skills/`. Each skill has a `SKILL.md` and optional `references/` folder.

Angular: `angular-component`, `angular-di`, `angular-directives`, `angular-forms`, `angular-http`, `angular-preferences`, `angular-routing`, `angular-signals`, `angular-ssr`, `angular-testing`, `angular-tooling`

Other: `code-review`, `commit-and-pr`, `css`, `ionic`, `nestjs`, `nestjs-config`, `nestjs-logging`, `orchestrate-dev-team`, `rxjs`, `typescript`

## Agents

Located in `agents/`. Specialist roles: `committer`, `implementer`, `linter`, `planner`, `release-notes-writer`, `researcher`, `reviewer`, `unit-tester`
