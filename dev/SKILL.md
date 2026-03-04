---
name: dev
description: Create high-clarity implementation prompts for a predefined chunk of engineering work. Use when a user already has a selected chunk and wants a prompt that instructs a coding agent to implement it with exact scope, constraints, acceptance criteria, tests, and done conditions.
---

# Dev

## Goal

Turn a chosen chunk into one execution-ready implementation prompt that minimizes ambiguity and scope drift.

## Inputs

Collect the following inputs before drafting the final prompt:

- `chunk statement`: one-sentence definition of the predetermined work chunk
- `repo context`: relevant modules, directories, or files
- `constraints`: compatibility, performance, security, style, and non-goals
- `acceptance criteria`: what must be true when done
- `verification commands`: exact tests, linters, or checks to run

If details are missing, add an `Assumptions` section with only minimal, explicit assumptions.

## Workflow

1. Restate the chunk and boundaries.
- Write one sentence beginning with `Implement ...`.
- List in-scope items and explicit out-of-scope items.

2. Convert requirements into implementation steps.
- Produce a 4 to 8 step ordered plan.
- Name likely files or modules to touch.
- Call out risky areas and compatibility constraints.

3. Define the verification bar.
- Specify exact commands for tests and checks.
- Include key behavioral assertions and regressions to avoid.
- Include blocked-path guidance when prerequisites are missing.

4. Draft the final implementation prompt.
- Keep language direct, specific, and execution oriented.
- Include a strict output contract for what the coding agent must report.

## Prompt Output Contract

Return the final prompt with these sections in order:

1. `Objective`
2. `Context`
3. `Scope`
4. `Out of Scope`
5. `Implementation Plan`
6. `Constraints`
7. `Acceptance Criteria`
8. `Verification`
9. `Deliverables`
10. `Reporting Format`

## Guardrails

- Do not widen scope beyond the predefined chunk.
- Do not leave verification vague or optional.
- Prefer concrete file paths, commands, and measurable criteria.
- Avoid architecture redesign unless explicitly requested.
- State uncertainty directly and keep assumptions minimal.

## Example Triggers

- "Use $dev to write the implementation prompt for this chunk."
- "Turn this scoped work item into a coding prompt."
- "Draft a build prompt for this predetermined engineering slice."
