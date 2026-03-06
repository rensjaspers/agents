---
name: pr-without-approval
description: Handles final git commit and pull request flow without user confirmation. Use when the pipeline is fully autonomous and no human-in-the-loop is needed.
---

# PR Without Approval

Use this skill at the end of an autonomous pipeline when version control actions must happen without pausing for user input.

## Workflow

1. Inspect current git state
   - Run `git status`, `git diff`, and recent `git log` to understand pending changes and local commit style.

2. Stage and commit
   - Stage only relevant files.
   - Draft commit message in simple English:
     - Keep title short.
     - Put context and reasoning in body.
   - If the project uses a required format (for example Conventional Commits), follow it.
   - Create the commit immediately without asking for confirmation.

3. Push and open PR
   - Verify branch state and push branch.
   - Draft PR title and body (summary + test plan).
   - Create the PR with `gh pr create` immediately without asking for confirmation.
   - Return the PR URL.

## Safety Rules

- Never run destructive git commands.
- Never force push.
- Never include secret files in commit (for example `.env`, key files, credentials). Before staging, verify no secrets are included by checking that files matching patterns like `*.env`, `*.key`, `*.pem`, `*.p12`, `*.pfx`, or names like `secrets.*` are excluded, and confirm sensitive paths are covered by `.gitignore`.

## Output Format

- Actions taken
- Commit hash and message
- PR URL
- Warnings or blockers
