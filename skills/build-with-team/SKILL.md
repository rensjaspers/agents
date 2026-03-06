---
name: build-with-team
description: Build a feature or fix a bug using a team of specialist subagents. Fully autonomous — no human-in-the-loop from start to finish. Use for tasks of medium complexity that need research, planning, implementation, testing, linting, review, and release notes with explicit handoffs.
---

# Build With Team

Use this skill for tasks of medium complexity: tasks that require multiple specialist steps but have a well-defined goal and limited unknowns. Examples: adding a feature to an existing module, fixing a reproducible bug, or refactoring a bounded part of the codebase. For exploratory or high-risk tasks where human judgment is needed mid-way, use a different approach.

This pipeline runs fully autonomously from start to finish — there is no human-in-the-loop at any stage.

## Goal

Complete complex work with a coordinated subagent pipeline:
scout -> planner -> implementer -> unit-tester -> linter -> reviewer -> release-notes-writer -> committer.

## Step-by-Step Process

1. Start `scout`
   - Gather all available context from external sources: tickets, bug reports, user input, documentation.
   - Use context-hub MCP tools when available.
   - Ask user follow-up questions for missing context.
   - Do **not** diagnose root causes or propose technical solutions.
   - Output a clear, well-framed task description with all surrounding context.

2. Start `planner`
   - Receive the task description from the scout.
   - Analyse the codebase to understand what needs to change and how.
   - Build a detailed execution plan grounded in actual code analysis.
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
   - Use the `pr-without-approval` skill.
   - Commit and open a PR immediately without asking the user for confirmation.
   - Return commit hash and PR URL.

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
