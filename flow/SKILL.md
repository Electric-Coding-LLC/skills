---
name: flow
description: Orchestrate an autonomous delivery flow by chaining `chunk`, implementation, `slop`, `review`, `yeet`, and `yoink`. Use when a user wants the agent to take scoped work forward with minimal supervision from implementation through PR creation and post-merge cleanup.
---

# Flow

## Goal

Execute one delivery cycle from scoped chunk selection through cleanup, review, PR creation, and PR finalization. Treat invocation of `$flow` as authorization to run `$yeet` and `$yoink` once their technical gates are satisfied.

## Autonomy Policy

Default behavior under `$flow`:

- Proceed autonomously through chunk selection, implementation, cleanup, and review.
- If the repo already has a plan doc, checklist, or progress tracker for the current task, keep it updated as work advances.
- Make reasonable local decisions without pausing when the scope, repo patterns, and verification path are clear.
- Keep the user informed with concise progress updates, but do not ask for confirmation at each stage.

Pause and ask only when one of these applies:

- Product intent or required behavior is ambiguous in a way that could change the implementation materially.
- The work reveals a risky cross-cutting change outside the selected chunk.
- Required credentials, services, permissions, or missing environment prerequisites block safe execution.
- Review finds a tradeoff that needs user preference rather than engineering judgment.

## Workflow

0. Check for an existing progress artifact.
- Look for a repo-local plan doc, checklist, or task tracker that clearly matches the current work.
- Prefer the most specific existing artifact over creating a new one.
- Do not create a new plan doc unless the user explicitly asks for one.
- If one exists, update it at each meaningful stage transition with factual status, not aspirational prose.

1. Run `$chunk`.
- Identify the next moderate, context-safe implementation chunk.
- Capture definition of done, exact repo-native verification commands for the chunk, and out-of-scope boundaries.
- If the user already provides a preselected chunk, treat that as the chunk output and continue.
- If `$chunk` returns `No justified next chunk`, `measure first`, `blocked`, or `done for now`, stop the flow there and report that outcome instead of pushing into implementation.
- If a progress artifact exists, record the selected chunk or stop condition before moving on.

2. Execute the implementation.
- Implement only what `$chunk` defines or what the user already scoped.
- Keep changes tight to scope, run targeted checks while coding, and finish by running the repo-native local equivalent of required CI checks for the changed scope before review.
- If a reusable handoff artifact is explicitly requested, draft that separately instead of widening this flow.
- Prefer finishing a complete, reviewable increment over partial exploratory edits.
- If a progress artifact exists, update implementation status after meaningful milestones, not every tiny edit.

3. Run `$slop`.
- Audit the changed areas for unnecessary fallbacks, stale guards, compat leftovers, and workaround hacks.
- Remove or replace slop before final review.
- If a progress artifact exists, note any scope cuts, cleanup decisions, or remaining known risks that affect the plan.

4. Run `$review`.
- Perform code, security, and style passes on the current diff.
- If blocking issues remain and are locally fixable, fix them and repeat `$review` until `Ready for Yeet: yes`.
- If review is blocking because required checks are unavailable due to missing credentials, services, tools, or other environment prerequisites, stop and report the blocked prerequisite instead of retrying review in a loop.
- If a progress artifact exists, record review status, blocking findings, and check outcomes.

5. Run `$yeet`.
- Proceed automatically when review is clean.
- Stage, commit, push, and open the PR without asking for extra confirmation.
- If a progress artifact exists, record branch/PR state once opened.

6. Run `$yoink`.
- Proceed automatically after `$yeet` and continue through actual merge completion.
- If checks are pending, keep waiting and polling until the PR is merged or a real blocker appears.
- Auto-merge may be enabled when that is the safe path, but enabling auto-merge is not completion and does not satisfy `$yoink` by itself.
- Verify checks, merge safely, and clean up local and remote branch state without asking for separate permission.
- If a progress artifact exists, record the final merged or blocked state and any next-step handoff.

## Stage Handoff Contract

Do not advance stages unless the previous stage produced the required outputs:

- `progress artifact` when present: current stage/status reflected accurately enough that another agent could resume from it.
- `$chunk`: selected chunk, rationale, done criteria, checks, out-of-scope, and stop condition.
- `implementation`: completed code changes plus exact commands and outcomes for required local checks.
- `$slop`: blocking/non-blocking slop findings, confirmed keeps, slop verdict.
- `$review`: blocking/non-blocking findings, checks run, explicit yeet readiness.
- `$yeet`: branch name, commit, and PR URL.
- `$yoink`: PR number and URL, check outcome, confirmed merge result, and cleanup actions.

## Final Reporting Contract

When `$flow` completes or pauses, report:

- `Outcome`: what was completed or why execution stopped.
- `Changes made`: concise summary of the implemented behavior or files touched.
- `Checks run`: exact verification performed and any skipped checks with reason.
- `Open risks or blockers`: only unresolved items that materially affect next steps.
- `Next action`: either what the user should decide or what technical blocker is preventing `$yeet` or `$yoink`.

## Guardrails

- Keep order strict: `$chunk -> implementation -> $slop -> $review -> $yeet -> $yoink` when later stages are in scope.
- Prefer updating an existing plan doc/checklist over duplicating the same progress in a second repo-local artifact.
- Keep plan-doc edits terse and factual; do not rewrite the whole document just to mark progress.
- If the existing plan doc is clearly stale or partially wrong, correct only the parts needed to make current status legible.
- If `$chunk` says there is no justified implementation chunk, stop. Do not reinterpret that as permission to find something else.
- Do not widen scope beyond the selected chunk unless the user explicitly expands scope.
- Do not treat "a check ran" as sufficient when the repo defines a closer local equivalent of the PR gate.
- Do not advance past implementation or run `$yeet` while required local checks are failing, skipped, or still undiscovered in a repo that defines them.
- If required checks are blocked by unavailable local prerequisites, stop and report the blocker instead of looping on `$review`.
- Do not run `$yeet` while blocking review issues remain.
- Do not treat "auto-merge enabled" as equivalent to "merged."
- Do not consider `$yoink` complete until the PR is actually merged.
- If CI is pending and no external blocker exists, keep waiting instead of stopping after enabling auto-merge.
- If dependencies or context are missing, pause and request only the minimum missing input.
- Do not stop for routine implementation choices that can be resolved from local code and repo conventions.
- Do not confuse status updates with permission checks.
- Prefer explicit reporting of assumptions over silent scope drift.
- Prefer stopping over speculative optimization when measurement or a new requirement is the real next step.

## Example Triggers

- "Use `$flow` to ship the next chunk end-to-end."
- "Run `$flow` for chunk selection, implementation, cleanup, review, and PR creation."
- "Take this from scoped work to merged PR with `$flow`."
- "Use `$flow` to implement this, open the PR, then merge and clean up once checks pass."
