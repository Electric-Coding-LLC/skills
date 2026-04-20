---
name: superflow
description: Lead a release or project slice from theme discovery through execmap planning, optional UI design and wireframes, implementation, cleanup, final review, and PR merge via `sendit`. Use when the user wants one supervised flow to turn a current goal into shipped code without running release or publish scripts.
---

# Superflow

## Goal

Take a current release, project, or theme from "what are we trying to ship?" to a merged pull request.

`$superflow` is a top-level orchestration skill:

- gather the real current goal and constraints
- run the work under `$supervisor`
- front-load planning with `$execmap`
- add UI design and ASCII wireframes when screens are part of the scope
- complete implementation
- run `$slop` and fix blocking findings
- run `$review` and fix blocking findings
- run `$sendit`

Release and publish steps stay manual unless the user explicitly expands scope.

## Use When

- The user wants to move a release or project theme forward end-to-end.
- The work is large enough to need planning, execution, review, and delivery.
- UI screens may be involved and should be designed before implementation.
- The user wants one coordinated flow instead of manually invoking each skill.

Do not use `$superflow` for:

- trivial one-file edits
- isolated bug fixes that do not need planning
- release publishing or store submission steps

## Completion Contract

Default definition of done under `$superflow`:

1. The current goal is clarified enough to execute safely.
2. An `execmap` exists or has been updated to match the real work.
3. Any relevant UI direction and wireframes exist before UI implementation starts.
4. The mapped implementation is complete for the selected slice.
5. `$slop` findings are resolved or intentionally kept with rationale.
6. `$review` is clean enough for shipping.
7. `$sendit` completes the PR and merge flow.
8. Manual release or publish scripts, if any, are left to the user.

If the user sets a narrower finish line, follow that instead.

## Workflow

1. Gather context first.
- Identify the current release theme, project goal, or scoped milestone.
- Read the repo's active plan, release doc, milestone doc, or equivalent source of truth before reframing the work.
- Inspect current repo state, open diffs, and any existing progress artifact.
- State the practical completion contract briefly, including what is explicitly out of scope.

2. Switch into `$supervisor` mode for execution.
- Treat `$superflow` as authorization to use `$supervisor` as the main control loop.
- Prefer fresh workers for bounded implementation or exploration steps when that reduces context pressure.
- Keep git, PR, and final delivery decisions with the supervisor.
- Maintain one compact durable progress artifact when the task is long enough to risk context drift.

3. Build or normalize planning with `$execmap`.
- Before substantial implementation, use `$execmap` to create or update the execution map.
- Prefer one initiative folder with one `EXECMAP.md` as the source of truth.
- Add step docs only when a step needs more definition.
- Keep completion state truthful. Do not mark steps complete until exit criteria are actually true.

4. Run the UI track when screens exist.
- Decide whether the scoped work includes screens, views, major UI states, or a design-system seam.
- If yes, use `$design` before UI implementation to define or sharpen the visual direction.
- If an existing design system exists, align with it instead of inventing a parallel one.
- Define concise text descriptions for each screen or view that matters to the slice.
- Use `$wirefmt` to create ASCII wireframe artifacts for those screens or views.
- Store design decisions and wireframes with the planning artifacts or the most relevant repo-local docs.

5. Execute the mapped implementation.
- Follow the next unchecked `execmap` item.
- Keep scope aligned to the selected slice instead of opportunistically widening the project.
- Update the execution map when sequence or scope changes.
- Run targeted repo-native checks while implementing.

6. Run `$slop` and resolve findings.
- Use `$slop` on the changed scope after implementation is functionally complete.
- Remove or replace stale fallbacks, compatibility leftovers, unnecessary guards, and unblocker hacks.
- If suspicious code must stay, record why it is still necessary.

7. Run `$review` and resolve findings.
- Use `$review` on the integrated diff after `$slop` cleanup has landed.
- Treat skipped or unavailable required checks as blocking unless the repo has no local equivalent.
- Fix blocking findings and rerun `$review` until the result is ready for shipping or truly blocked.

8. Run `$sendit`.
- Once `$review` and `$slop` are in a good state, use `$sendit` to stage, commit, push, open the PR, wait through checks, merge safely, and clean up.
- Do not run release or publish scripts afterward unless the user explicitly asks.
- End by reporting what shipped and what manual release actions remain, if any.

## Context Intake Checklist

At the start of `$superflow`, gather only the context needed to execute:

- current goal, release theme, or milestone
- repo source-of-truth plan doc or tracker
- active branch and working tree state
- relevant constraints, non-goals, or deadline pressure
- whether the scoped work includes UI screens
- repo-native verification and delivery commands

If a key fact is ambiguous but low risk, infer it and label the assumption.
If the ambiguity would materially change implementation, pause and ask one concise question.

## Progress Record

When the work is multi-step or likely to outgrow one clean reasoning pass, keep a compact progress artifact with exactly:

- `Goal`
- `Done`
- `Next`
- `Checks`
- `Risks`
- `Delivery state`

Update it at meaningful stage boundaries:

- after context intake
- after `execmap` creation or revision
- after UI design/wireframe work
- after implementation milestones
- after `slop` and `review`
- after `sendit`

## Output Contract

When reporting `$superflow` progress or completion, return:

1. `Goal`
- Current release or project objective and definition of done.

2. `Plan`
- Ordered execution path, usually the current `execmap` track.

3. `Latest result`
- What the last completed stage or worker accomplished.

4. `Checks`
- Exact verification run and outcomes.

5. `Status`
- `continue`, `blocked`, or `done`

6. `Summary`
- Concise rollup of what is complete and what remains.

7. `Delivery state`
- local only, ready to commit, ready for PR, PR open, or merged

If the flow stops before `sendit`, state the precise blocker.
If it completes, clearly separate merged code delivery from any manual release or publish follow-up.

## Guardrails

- Do not skip context gathering and jump straight into implementation on non-trivial work.
- Do not start substantial implementation before `execmap` exists or has been updated for the current slice.
- Do not invent UI screens just to satisfy the UI track; only run it when screens or views are actually part of scope.
- Do not let design work drift into abstract product philosophy. Keep it screen-specific and execution-oriented.
- Do not hand-draw ASCII wireframes when `$wirefmt` should be used.
- Do not let step docs or status notes become a second source of truth over `EXECMAP.md`.
- Do not keep stale progress in docs; update the real tracker as work advances.
- Do not treat cleanup or review as optional. Fix blocking `$slop` and `$review` findings before `$sendit`.
- Do not treat "PR opened" or "auto-merge enabled" as completion when `$sendit` has not actually merged.
- Do not run publish, release, deploy, or store-submission scripts unless the user explicitly asks.

## Example Triggers

- "Use `$superflow` to take this release theme from planning through merge."
- "Run `$superflow` for this project slice, including UI planning if needed."
- "Use `$superflow` to build the execmap, implement the work, clean it up, review it, and ship the PR."
