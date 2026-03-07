---
name: planning
description: Analyse the codebase and produce a detailed, step-by-step execution plan from scout output or direct user input. Use after scouting is complete and before implementation starts.
---

# Planning

Use this skill to turn a validated, well-framed task description (from the scout) into a concrete execution plan that lower-effort models can follow reliably.

You are the **code expert** in this pipeline. The scout has clarified the question; your job is to understand the codebase deeply enough to determine _how_ to answer it, and then produce an unambiguous plan.

## Goal

Produce a detailed plan with clear steps, dependencies, and verification checks, so implementers can execute without ambiguity.

## How to Work

### Phase 1 — Understand the task

1. Read the scout output (or user input) carefully.
2. Clarify objective, scope, and constraints from the provided context.
3. Identify which parts of the codebase are likely involved.

### Phase 2 — Analyse the code

4. Explore the relevant files, modules, and entry points.
5. Map out existing patterns, abstractions, and conventions used in the project.
6. Identify dependencies, side-effects, and risk areas.
7. Determine the root cause (for bugs) or the integration points (for features).
8. Confirm or challenge assumptions from the scout output based on what you find.

### Phase 3 — Write the plan

9. Break work into ordered steps with clear dependencies.
10. Identify which steps can be executed in parallel by implementers and mark them explicitly.
11. Define expected file changes, tests, and verification checks per step.
12. Add risk notes and fallback options for fragile steps.
13. Keep steps concrete, small, and actionable.

## Inputs

- Prefer scout output (validated, well-framed task description with context) when available.
- If scout output is missing, use the user input directly.

## Output Format

- **Objective**
- **Scope and non-goals**
- **Assumptions** (including any that differ from scout assumptions after code analysis)
- **Relevant code areas** (files, modules, patterns observed)
- **Step-by-step execution plan** (mark parallel-safe steps explicitly)
- **Verification plan**
- **Risks and mitigations**
- **Handoff notes for implementer**
