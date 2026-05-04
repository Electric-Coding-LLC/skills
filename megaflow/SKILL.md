---
name: megaflow
description: Run a roadmap as a sequence of bounded version deliveries by selecting the next incomplete version, promoting it into active execution, running a fresh `superflow` for that one version, then re-reading repo truth before deciding whether to continue. Use when the user wants repeated version-by-version execution until the roadmap is complete without collapsing the whole roadmap into one flow.
---

# Megaflow

## Goal

Own the roadmap loop without owning version implementation directly.

`$megaflow` is a thin orchestration skill above `$superflow`:

- read roadmap and plan truth
- choose the next incomplete version-sized slice
- promote or activate that version for execution
- keep roadmap, `PLAN.md`, and promoted `EXECMAP.md` progress in sync at each loop boundary
- run a fresh `$superflow` for that one version
- inspect the repo again after that run finishes
- either continue to the next version or stop for a real reason

`$megaflow` is not a replacement for `$superflow`.
`$superflow` remains the delivery unit.

## Use When

- The repo has a real roadmap with multiple version-sized entries.
- The user wants multiple roadmap versions executed sequentially, not just the next one.
- The repo has a clear planning lifecycle such as roadmap -> active plan -> completed version history.
- It is useful to re-evaluate repo truth after each merged version before continuing.

Do not use `$megaflow` for:

- a single version or milestone
- work that has no roadmap or no version ordering
- one giant implementation push that should stay inside one `EXECMAP`
- automatic publish, deploy, or store-submission loops

## Completion Contract

Default definition of done under `$megaflow`:

1. The roadmap source of truth is identified.
2. The next incomplete version is selected from real repo state.
3. That version is promoted into active execution if needed, and the progress artifacts reflect that promotion immediately.
4. A fresh `$superflow` runs for that one version and keeps owning it until delivery is actually merged or truly blocked.
5. Progress artifacts stay current at each loop boundary instead of being deferred to a final repo recheck.
6. After the version is merged, roadmap and plan truth are re-read and normalized before any continuation.
7. Intermediate delivery states such as `local only`, `ready for PR`, or `PR open` do not count as version completion.
8. The loop continues only while the next version is still well-defined and safe to promote.
9. The flow stops immediately when a real blocker, manual handoff, or planning mismatch appears.

If the user sets a narrower finish line, follow that instead.

## Workflow

1. Gather roadmap truth first.
- Read the roadmap, active plan, version docs, and shipped-state source of truth before choosing any version.
- Confirm how the repo expresses:
  - planned versions
  - active execution
  - completed roadmap versions
  - public package or app releases, if any
- State the practical completion contract briefly, including what should cause the loop to stop.

2. Select one version at a time.
- Identify the next incomplete version-sized slice from the roadmap's real ordering and status.
- Prefer the roadmap's explicit next incomplete entry over inventing a new sequence.
- If the roadmap is ambiguous, stale, or contradictory, stop and resolve that instead of guessing.
- Do not own multiple versions concurrently.

3. Promote the selected version into execution.
- If the repo requires promotion from roadmap into an active plan or `EXECMAP`, do that before substantial implementation starts.
- Keep roadmap status, active plan status, and execution state aligned without inventing a second tracker.
- Treat the promoted version as the only active execution unit for the current loop.
- Run a progress sync pass immediately after promotion so the outer-loop docs already match the selected version.

4. Run a fresh `$superflow`.
- Use a fresh subagent or fresh execution context for the selected version whenever possible.
- Brief `$superflow` with only the selected version, relevant repo truth, and current constraints.
- Let `$superflow` own planning, optional design, implementation, cleanup, review, and delivery for that one version.
- Tell `$superflow` to keep the current version's progress artifacts updated as part of the active work, not as wrap-up.
- If `$superflow` stops in an intermediate delivery state such as `local only`, `ready for PR`, or `PR open`, do not treat the version as finished. Continue driving the same version until delivery is `merged` or truly `blocked`.
- Do not widen the delegated scope to include later roadmap versions.

5. Re-read and normalize repo truth after each version.
- After the selected version is actually merged, inspect the repo again before deciding what is next.
- Confirm whether the version actually landed, whether the roadmap was updated truthfully, and whether a new active plan is required.
- Treat post-merge bookkeeping as active loop work, not as a separate follow-up task.
- If the post-merge roadmap or active-plan state is stale but mechanically obvious to fix, normalize it as part of the loop instead of stopping.
- Typical housekeeping includes marking the finished version `completed`, clearing the active `PLAN.md` when appropriate, and identifying the next justified version without promoting it until the loop explicitly continues.
- Re-check shipped-state sources, plan docs, and roadmap status rather than trusting stale pre-run assumptions.

6. Decide whether to continue or stop.
- Continue only if:
  - the previous version is actually merged
  - the roadmap still has a clear next incomplete version
  - the next version should still exist with roughly the same scope
- Run a progress sync pass before any continue/stop decision so bookkeeping does not fall out of memory between loop iterations.
- Normalize stale-but-obvious post-merge state before deciding to stop.
- Otherwise stop and report the exact reason.

## Stop Conditions

Stop `$megaflow` immediately when any of these are true:

- the roadmap does not clearly identify the next version
- the roadmap and active plan disagree about execution state in a way that is not mechanically obvious to normalize
- `$superflow` ends `blocked`
- `$superflow` cannot advance the current version beyond an intermediate delivery state without new user input or missing prerequisites
- the just-finished version changes the priority or scope of the next version
- the repo no longer has a justified next promoted version
- credentials, services, or other environment prerequisites block safe continuation

Stopping is correct behavior.
Do not convert uncertainty into forced progress.

## Stage Handoff Contract

Do not advance the roadmap loop unless the current stage produced the needed output:

- `roadmap intake`: identified roadmap source, active-plan source, shipped-state source, and stop conditions
- `version selection`: one concrete next version and why it is next
- `promotion`: active execution artifact created or normalized for that version, with roadmap and `PLAN.md` synced
- `superflow run`: clear outcome for the selected version, including delivery state
- `post-run recheck`: updated and, when obvious, normalized roadmap, plan, and internal completion state after merge
- `continuation decision`: explicit `continue`, `stop`, or `blocked`

## Progress Record

When the loop is long enough to risk context drift, keep one compact record
with exactly:

- `Current version`
- `Done`
- `Next`
- `Checks`
- `Delivery state`
- `Progress sync`

`Progress sync` should record:

- the last roadmap, `PLAN.md`, or `EXECMAP.md` update that landed
- the next required normalization or promotion update
- whether any tracker drift is still outstanding

Refresh it:

- after roadmap intake
- after promotion
- after any `superflow` milestone that changes delivery state
- after merge
- after post-merge normalization
- before any continue/stop/done report

## Output Contract

When reporting `$megaflow` progress or completion, return:

1. `Roadmap`
- Source of truth and current version ordering used for decisions.

2. `Current version`
- The version currently selected or just completed.

3. `Latest result`
- What the last `$superflow` run accomplished.

4. `Progress sync`
- What planning or progress artifacts were updated during the latest loop boundary.

5. `Repo recheck`
- What changed in roadmap, active plan, and internal completion state after the current version was merged, including any normalization performed.

6. `Loop status`
- `continue`, `stop`, `blocked`, or `done`

7. `Reason`
- Exact reason for continuing or stopping.

8. `Next version`
- The next version only if it is still justified after the recheck.

## Guardrails

- Do not turn one `$megaflow` run into one giant multi-version implementation blob.
- Do not skip the repo recheck between versions.
- Do not carry stale assumptions about the roadmap forward after a version lands.
- Do not run the next version just because the roadmap file exists.
- Do not treat a roadmap entry as executable until it has been promoted into the repo's active execution artifact when the repo requires that step.
- Do not let `$megaflow` replace `$superflow`; delivery remains inside `$superflow`.
- Do not treat `local only`, `ready for PR`, `PR open`, or `auto-merge enabled` as equivalent to a finished version.
- Do not evaluate the next roadmap version while the current one is still unmerged.
- Do not stop just because public release or publish work remains manual; roadmap completion is internal.
- Do not stop on stale post-merge bookkeeping when the normalization step is straightforward and repo-local.
- Do not rely on the final repo recheck to remember progress updates that should have happened during promotion, merge, or handoff.
- Do not leave mechanically obvious roadmap, `PLAN.md`, or promoted `EXECMAP.md` updates for the user when they arose from the current loop.
- Do not make a continue/stop decision until a progress sync pass has run for that loop boundary.
- Do not keep going when the next version should be re-scoped based on what just landed.
- Do not invent a second long-lived tracker when the repo already has roadmap and active-plan artifacts.

## Example Triggers

- "Use `$megaflow` to work through this roadmap one version at a time."
- "Run `$megaflow` until the roadmap is complete, treating each merged version as internal completion."
- "Use `$megaflow` to promote the next version, run `$superflow`, then decide whether the next roadmap version is still justified."
- "Run `$megaflow` on this repo's roadmap, but stop whenever the roadmap and plan drift or a version finishes blocked."
