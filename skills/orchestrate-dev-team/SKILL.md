---
name: orchestrate-dev-team
description: Orchestrate a development team of subagents. Use when a task needs research, planning, implementation, testing, linting, review, and release notes with explicit handoffs.
---

# Orchestrate Dev Team

Use this skill when a request is large, risky, or multi-step.

## Goal

Complete complex work with a coordinated subagent pipeline:
researcher -> planner -> implementer -> unit-tester -> linter -> reviewer -> release-notes-writer -> committer.

## Step-by-Step Process

1. Start `researcher`
   - Collect facts only (no guessing).
   - Use context-hub MCP tools when available.
   - Ask user follow-up questions for missing context.
   - Output a complete planning brief.

2. Start `planner`
   - Build a detailed execution plan from researcher output (or user input if needed).
   - Make steps concrete so lower-effort models can execute reliably.
   - Include verification steps, risks, and fallback options.

3. Start `implementer`
   - Implement only after a concrete plan exists.
   - Execute the plan step by step.
   - Follow `AGENTS.md` coding and review guidelines strictly.
   - Delegate simple support tasks to subagents when useful.

4. Start `unit-tester`
   - Add/update tests for new logic.
   - Only change test code.
   - If failures suggest implementation bugs, return findings to implementer.

5. Start `linter`
   - Run lint/lint-fix if available.
   - Allow safe automated formatting/import fixes.
   - Return non-automatable issues to implementer.

6. Start `reviewer`
   - Run code review using the `code-review` skill.
   - Report concrete findings to implementer.

7. Iterate until clean
   - If tester/linter/reviewer reports issues, implementer evaluates feedback critically.
   - Implementer applies only changes that truly improve code quality.
   - Re-run tester/linter/reviewer as needed.

8. Start `release-notes-writer`
   - Run at the end after implementation is accepted.
   - If a release-notes skill exists, use it.

9. Start `committer`
   - Run only after release notes are ready.
   - Use the `commit-and-pr` skill.
   - First ask user choice: no commit, commit only, or commit + PR.
   - Execute only the selected path and return commit hash / PR URL when relevant.

## Handoff Contract

For every handoff, include:
- Objective
- Current state
- Evidence (logs, test output, lint output, review findings)
- Open risks
- Exact next action

## Hard Rules

- No implementation without a concrete plan.
- No fabricated facts. Unknowns must be stated explicitly.
- Reviewer/tester/linter feedback is input, not automatic truth.
- Keep all changes traceable to objective and plan.
