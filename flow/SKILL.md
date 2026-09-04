---
name: flow
description: Take one scoped implementation chunk through implementation, review, and GitHub PR merge. Use when the user wants autonomous delivery of a bounded change; use superflow for a full planned delivery unit and deliver for production publication of a ready change.
---

# Flow

Complete one reviewable increment through a merged PR and branch cleanup. An explicit `$flow` request authorizes that GitHub delivery unless the user sets a narrower finish line. Selecting a skill from context does not expand the user's authorization. Production publication is a separate scope; recognize any deployment automatically triggered by merge.

## Workflow

1. Establish the chunk.
- Read the task, repository instructions, current branch and changes, and any relevant existing plan.
- Use `$chunk` unless the user already supplied a bounded implementation unit.
- Capture the outcome, acceptance criteria, repo-native checks, and scope boundaries.
- If the result is `No justified next chunk`, `measure first`, `blocked`, or `done for now`, report it without inventing implementation work.

2. Implement the selected scope.
- Make a complete, coherent change and run relevant checks. Preserve unrelated work.
- Keep any existing task plan current at meaningful milestones; do not create a new tracker solely to run this flow.
- Resolve routine choices from repository evidence. Ask only when missing intent, access, or a material tradeoff prevents safe progress.

3. Run `$review` on the complete intended delivery diff.
- Fix actionable blocking findings and rerun affected checks or review when the content changes.
- Reuse still-valid verification evidence. Stop on an external prerequisite that cannot be resolved within the task instead of repeating the same blocked review.

4. Run `$sendit`.
- Let `$sendit` own the final pre-staging progress sync, commit, push, PR, checks, merge, and branch cleanup.
- Pass the selected scope, review evidence, and existing authorization to it.
- Continue until the PR is actually merged or a concrete blocker requires user action. An open PR or enabled auto-merge is not completion.

## Boundaries and reporting

Do not widen the chunk without user direction or treat routine progress updates as approval checks. Required repository checks must pass before delivery; a lighter ad hoc check does not replace a defined gate.

Report the behavior changed, verification and any material gap, PR/merge result, cleanup, and remaining blocker or next action when one exists. Final GitHub merge state belongs in the report; do not create a second docs-only PR merely to record it.
