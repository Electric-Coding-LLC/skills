---
name: flow
description: Orchestrate an end-to-end delivery flow by chaining `chunk`, `dev`, `slop`, `review`, and `yeet` in order. Use when a user wants to go from next-chunk selection to implementation hardening and PR creation in one guided sequence.
---

# Flow

## Goal

Execute one delivery cycle from scoped chunk selection to opened PR, with explicit quality gates between each stage. Treat invocation of `$flow` as explicit permission to run `$yeet` after review passes.

## Workflow

1. Run `$chunk`.
- Identify the next moderate, context-safe implementation chunk.
- Capture definition of done, verification checks, and out-of-scope boundaries.
- If the user already provides a preselected chunk, treat that as the chunk output and continue.

2. Run `$dev`.
- Convert the chosen chunk into an execution-ready implementation prompt.
- Lock scope to the chunk and include exact acceptance criteria and verification commands.

3. Execute the implementation.
- Implement only what `$dev` defines.
- Keep changes tight to scope and run targeted checks while coding.

4. Run `$slop`.
- Audit the changed areas for unnecessary fallbacks, stale guards, compat leftovers, and workaround hacks.
- Remove or replace slop before final review.

5. Run `$review`.
- Perform code, security, and style passes on the current diff.
- If blocking issues remain, fix them and repeat `$review` until `Ready for Yeet: yes`.

6. Run `$yeet`.
- Proceed automatically when review is clean. Do not ask for extra confirmation after `$flow` is invoked.
- Stage, commit, push, and open a draft PR following the yeet workflow.

## Stage Handoff Contract

Do not advance stages unless the previous stage produced the required outputs:

- `$chunk`: selected chunk, rationale, done criteria, checks, out-of-scope.
- `$dev`: objective, scoped plan, constraints, acceptance criteria, verification.
- `implementation`: completed code changes plus executed checks.
- `$slop`: blocking/non-blocking slop findings, confirmed keeps, slop verdict.
- `$review`: blocking/non-blocking findings, checks run, explicit yeet readiness.
- `$yeet`: branch name, commit, and PR URL.

## Guardrails

- Keep order strict: `$chunk -> $dev -> implementation -> $slop -> $review -> $yeet`.
- Treat `$flow` invocation as explicit authorization for `$yeet`; do not require separate confirmation language.
- Do not widen scope beyond the selected chunk unless the user explicitly expands scope.
- Do not run `$yeet` while blocking review issues remain.
- If dependencies or context are missing, pause and request only the minimum missing input.

## Example Triggers

- "Use `$flow` to ship the next chunk end-to-end."
- "Run `$flow` for chunk, dev prompting, cleanup, review, and PR creation."
- "Take this from scoped work to draft PR with `$flow`."
