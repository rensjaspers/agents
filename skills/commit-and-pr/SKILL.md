---
name: commit-and-pr
description: Handles final git commit and pull request flow with user confirmation. Use when work is done and the user may want no commit, commit only, or commit plus PR.
---

# Commit And Pull Request

Use this skill at the end of implementation when version control actions are needed.

## Required First Step

Always ask the user first:
- no commit
- commit only
- commit + PR

Do not proceed before the user picks one option.

## Workflow

1. Inspect current git state
   - Run `git status`, `git diff`, and recent `git log` to understand pending changes and local commit style.

2. If user chose `no commit`
   - Stop after confirming no git write actions were taken.

3. If user chose `commit only` or `commit + PR`
   - Stage only relevant files.
   - Draft commit message in simple English:
     - Keep title short.
     - Put context and reasoning in body.
   - If the project uses a required format (for example Conventional Commits), follow it.
   - **Show the user the full commit message** (title + body) and ask for confirmation.
   - Do not run `git commit` until the user confirms.
   - After confirmation, create the commit.

4. If user chose `commit + PR`
   - Verify branch state and push branch if needed.
   - Draft PR title and body (summary + test plan).
   - **Show the user the full PR text** (title + body) and ask for confirmation.
   - Do not run `gh pr create` until the user confirms.
   - After confirmation, create the PR with `gh pr create`.
   - Return the PR URL.

## Safety Rules

- Never commit or open PR without explicit user choice.
- Never run destructive git commands.
- Never force push unless the user explicitly requests it.
- Never include secret files in commit (for example `.env`, key files, credentials).
- **Always show commit/PR text and wait for user confirmation before executing.**

## Output Format

- User choice
- Commit status (and hash if created)
- PR status (and URL if created)
- Warnings or blockers
