---
name: superflow
description: Lead an active roadmap version, promoted EXECMAP, or project delivery unit from theme discovery through execmap planning, optional UI design, implementation, cleanup, final review, and PR merge via `sendit`. Use when the user wants one supervised flow to complete the active EXECMAP or current goal without running release or publish scripts.
---

# Superflow

## Goal

Take a current roadmap version, active `EXECMAP.md`, project, or theme from "what are we trying to ship?" to a merged pull request.

`$superflow` is a top-level orchestration skill:

- gather the real current goal and constraints
- run the work under `$supervisor`
- front-load planning with `$execmap`
- keep roadmap, `PLAN.md`, `EXECMAP.md`, and other progress artifacts synced as work advances
- add UI design direction when screens are part of the scope and wireframes only when they materially clarify execution
- complete the active execmap delivery unit
- run cleanup-aware `$review` and fix blocking findings
- run `$sendit`

Release and publish steps stay manual unless the user explicitly expands scope.

## Use When

- The user wants to move a roadmap version or project theme forward end-to-end.
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
3. Any relevant UI direction exists before UI implementation starts, with wireframes added only when needed to clarify layout, flow, or states.
4. The mapped implementation is complete for the active execmap delivery unit.
5. Repo progress artifacts stay truthful at stage boundaries instead of being left for wrap-up.
6. `$review` is clean enough for shipping, including its slop pass.
7. `$sendit` completes the PR and merge flow.
8. Manual public release or publish scripts, if any, are left to the user.

When an `EXECMAP.md` already exists for the current goal, that file is the
default delivery unit. Do not shrink `$superflow` to the next unchecked item or
an arbitrary sub-slice unless the user explicitly sets that narrower finish
line or the execmap clearly spans multiple independent deliveries.

If the user sets a narrower finish line, follow that instead.

## Workflow

1. Gather context first.
- Identify the current roadmap version, active `EXECMAP.md`, project goal, or scoped milestone.
- Read the repo's active plan, roadmap, release doc, milestone doc, or
  equivalent source of truth before reframing the work.
- If a roadmap exists and the current version is not already active, use the
  roadmap's version ordering and status to identify the selected version-sized
  delivery unit.
- Inspect current repo state, open diffs, and any existing progress artifact.
- State the practical completion contract briefly, including what is explicitly out of scope.

2. Switch into `$supervisor` mode for execution.
- Treat `$superflow` as authorization to use `$supervisor` as the main control loop.
- Prefer fresh workers for bounded implementation or exploration steps when that reduces context pressure.
- Keep git, PR, and final delivery decisions with the supervisor.
- Maintain one compact durable progress artifact when the task is long enough to risk context drift.
- Keep one live `progress sync` obligation in view throughout the run so plan-state updates do not fall out of working memory.

3. Build or normalize planning with `$execmap`.
- Before substantial implementation, use `$execmap` to create or update the execution map.
- If the selected delivery unit came from a roadmap, promote that one version into
  `plans/<version>/EXECMAP.md` before substantial implementation starts.
- Prefer one initiative folder with one `EXECMAP.md` as the source of truth.
- Add step docs only when a step needs more definition.
- Keep any roadmap version-level status aligned with the promoted initiative,
  but keep step-level execution state in `EXECMAP.md`.
- Keep completion state truthful. Do not mark steps complete until exit criteria are actually true.
- Once an active `EXECMAP.md` exists for the current goal, treat its executable
  unchecked work as the delivery unit unless the user narrows scope or the map
  clearly covers multiple independent deliveries.
- After promotion or plan revision, run a progress sync pass immediately so roadmap, `PLAN.md`, and the active `EXECMAP.md` already reflect the chosen delivery unit.

4. Run the UI track when screens exist.
- Decide whether the scoped work includes screens, views, major UI states, or a design-system seam.
- If yes, use `$design` before UI implementation when the visual direction is ambiguous, high-impact, or part of the user's request.
- If an existing design system exists, align with it instead of inventing a parallel one.
- Define concise text descriptions for each screen or view that matters to the slice.
- Use `$wirefmt` to create ASCII wireframe artifacts only when layout, interaction flow, or state coverage would otherwise be unclear.
- Store durable design decisions, and any wireframes created, with the planning artifacts or the most relevant repo-local docs.

5. Execute the mapped implementation.
- Follow unchecked `execmap` items in order until the active delivery unit's exit criteria are satisfied.
- Keep scope aligned to the active `EXECMAP.md` or current delivery unit instead of opportunistically widening into unrelated roadmap work.
- Update the execution map when sequence or scope changes.
- Run targeted repo-native checks while implementing.
- After any milestone that changes real repo progress, run a progress sync pass instead of waiting until delivery is over.

6. Run `$review` and resolve findings.
- Use `$review` on the integrated diff after implementation is functionally complete.
- Treat its slop pass as part of the normal delivery gate: remove or replace stale fallbacks, compatibility leftovers, unnecessary guards, and unblocker hacks before shipping.
- If suspicious code must stay, record why it is still necessary.
- Treat skipped or unavailable required checks as blocking unless the repo has no local equivalent.
- Fix blocking findings and rerun `$review` until the result is ready for shipping or truly blocked.

7. Run `$sendit`.
- Once `$review` is in a good state, use `$sendit` to stage, commit, push, open the PR, wait through checks, merge safely, and clean up.
- Do not run release or publish scripts afterward unless the user explicitly asks.
- Before `$sendit` stages, run a final progress sync so repo-local planning artifacts reflect the implementation, verification, and ready-to-land state that should ship with the code.
- After PR creation and merge, report GitHub delivery state from the PR/check results; do not create a follow-up docs-only PR just to record that the PR merged.
- End by reporting what shipped and what manual release actions remain, if any.

## Context Intake Checklist

At the start of `$superflow`, gather only the context needed to execute:

- current goal, roadmap version, or milestone
- roadmap status when a roadmap is the version-level source of truth
- repo source-of-truth plan doc or tracker
- any existing progress artifact or stale-plan drift that must be normalized early
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
- `Progress sync`

Update it at meaningful stage boundaries:

- after context intake
- after `execmap` creation or revision
- after UI design work or wireframe work, when either changes tracker truth
- after implementation milestones
- after `review`
- before `$sendit` stages the delivery commit
- after `$sendit`, in the final report rather than a repo-doc edit, unless the user explicitly asked for a follow-up artifact
- before a handoff when tracker truth changed

`Progress sync` should record:

- which roadmap, `PLAN.md`, `EXECMAP.md`, or repo-local progress docs were updated last
- which progress update is due next
- whether any tracker drift still remains

## Progress Sync Pass

When a major stage changes tracker truth, reconcile the repo's progress artifacts
with the real state of the work.

Typical sync targets:

- roadmap status for the current version
- `PLAN.md` active/completed state
- active `EXECMAP.md` checkbox or scope truth
- any repo-local progress doc already used by the task

Default behavior:

- if the correct progress update is mechanically obvious and relevant to the
  current stage, make it now
- if no artifact state changed, say no sync was needed instead of editing docs
- if the correct update depends on an unresolved product decision, record the
  exact ambiguity and continue only as far as is safe
- make implementation-complete and ready-to-land tracker updates before
  `$sendit` stages, so they land in the same PR as the code
- treat final PR merged/blocked state as GitHub delivery truth for the final
  report, not as a reason for a second docs-only PR
- do not defer obvious progress updates until a final repo recheck, but avoid
  tracker churn for routine status reports

## Output Contract

When reporting `$superflow` progress or completion, return:

1. `Goal`
- Current version or project objective and definition of done.

2. `Plan`
- Ordered execution path, usually the current `execmap` track.

3. `Latest result`
- What the last completed stage or worker accomplished.

4. `Progress sync`
- What planning or progress artifacts were updated at the latest stage boundary.

5. `Checks`
- Exact verification run and outcomes.

6. `Status`
- `continue`, `blocked`, or `done`

7. `Summary`
- Concise rollup of what is complete and what remains.

8. `Delivery state`
- local only, ready to commit, ready for PR, PR open, or merged

If the flow stops before `sendit`, state the precise blocker.
If it completes, clearly separate merged code delivery from any manual release or publish follow-up.

## Guardrails

- Do not skip context gathering and jump straight into implementation on non-trivial work.
- Do not let one `$superflow` own the whole roadmap when the real unit of work
  is one promoted version.
- Do not reinterpret an active `EXECMAP.md` as only the next unchecked item; complete its mapped work unless the user sets a narrower finish line or the map clearly spans multiple independent deliveries.
- Do not start substantial implementation before `execmap` exists or has been updated for the current delivery unit.
- Do not treat a roadmap entry as executable work until it has been promoted
  into a versioned `EXECMAP`.
- Do not invent UI screens just to satisfy the UI track; only run it when screens or views are actually part of scope.
- Do not let design work drift into abstract product philosophy. Keep it screen-specific and execution-oriented.
- Do not create wireframes for narrow UI fixes where existing patterns and layout are already clear.
- Do not hand-draw ASCII wireframes when `$wirefmt` should be used.
- Do not let step docs or status notes become a second source of truth over `EXECMAP.md`.
- Do not keep stale progress in docs; update the real tracker as work advances.
- Do not rely on the final repo recheck to remember progress updates that became obvious earlier.
- Do not leave mechanically obvious roadmap, `PLAN.md`, or `EXECMAP.md` updates for the user when they arose from the current stage.
- Do not split implementation and final ready-to-land progress sync into separate PRs.
- Do not create a follow-up docs-only PR just to record final GitHub delivery state after merge.
- Do not let progress-sync work become a second tracker or verbose diary.
- Do not treat cleanup or review as optional. Fix blocking `$review` findings, including slop findings, before `$sendit`.
- Do not treat "PR opened" or "auto-merge enabled" as completion when `$sendit` has not actually merged.
- Do not run publish, release, deploy, or store-submission scripts unless the user explicitly asks.

## Example Triggers

- "Use `$superflow` to take this roadmap version from planning through merge."
- "Run `$superflow` to complete the active `EXECMAP.md`, including UI planning if needed."
- "Use `$superflow` to build the execmap, implement the work, run cleanup-aware review, and ship the PR."
- "Use `$superflow` on the current roadmap version after promoting it to `plans/0.5/EXECMAP.md`."
