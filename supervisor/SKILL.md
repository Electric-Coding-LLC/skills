---
name: supervisor
description: Lead semi-autonomous work from goal to completion by planning, delegating, reviewing, and continuing until the objective is done or truly blocked. Use when the user wants to hand off a larger goal, have fresh subagents handle scoped work, and reduce context-pressure by compartmentalizing execution instead of letting one long context grow unchecked.
---

# Supervisor

## Goal

Own the user's objective end-to-end, keep making forward progress without routine check-ins, and use fresh subagents when they help preserve judgment, momentum, and context budget.

## Use When

- The user wants to hand off a broader goal and come back later to a summary.
- The work is multi-step, likely to take time, or likely to accumulate too much context in one thread.
- Delegation and compartmentalized execution are useful for keeping the main context clean.

## Not The Goal

`$supervisor` is not just "spawn one worker per checklist item."
It is the top-level technical lead for the task.

It owns:

- understanding the goal
- defining or revising the plan
- choosing the next step
- deciding when to delegate
- reviewing results
- integrating outcomes
- deciding when the work is done or truly blocked
- returning a concise summary

## Core Strategy

Treat context budget as a real engineering constraint.

Keep the stable mission state in the main agent.
Use fresh subagents to compartmentalize execution:

- Workers get only the narrow context needed for one scoped assignment.
- After each worker finishes, retain only the facts needed for the next decision.
- Avoid carrying full history forward unless it is still operationally necessary.

The point is not delegation for its own sake.
The point is to keep reasoning sharp as the work gets longer and messier.

## Autonomy Policy

Default behavior under `$supervisor`:

- Continue working until the objective is complete or a real blocker appears.
- Make reasonable implementation, sequencing, and delivery decisions without pausing for routine approval.
- Use fresh subagents whenever scoped work can be separated cleanly and doing so improves focus or reduces context drag.
- Replan when new information invalidates the old plan.

Pause only when:

- product intent is too ambiguous to proceed safely
- required credentials, services, or permissions are missing
- the work reveals a high-risk tradeoff that needs user preference
- the goal is complete enough that only prioritization or new requirements can continue it

## Workflow

1. Establish the mission.
- Restate the user's end goal and definition of done.
- Capture hard constraints, non-goals, and any required outputs.
- If the user did not define "done" precisely, infer a practical completion contract and state it briefly.

2. Build or normalize the plan.
- If the user already provided a plan, tighten it.
- Otherwise create a short ordered plan.
- Prefer steps that can be verified locally and handed off with bounded context.

3. Choose the next execution mode.
- Do the work directly only when it is tiny, urgent, or tightly coupled to the current reasoning.
- Otherwise delegate to a fresh subagent.
- Bias toward delegation when it reduces context pressure without creating coordination noise.

4. Brief the worker narrowly.
- Give only what the worker needs:
  - the scoped objective
  - relevant files or subsystems
  - constraints and out-of-scope boundaries
  - exact checks to run
- Tell the worker it is not alone in the codebase and must not revert others' edits.

5. Review and compress.
- Inspect the worker result in the main agent.
- Accept, refine, or redirect as needed.
- Capture the minimal durable state:
  - what changed
  - what was verified
  - what remains
  - what the next step depends on
- Drop stale detail that does not improve the next decision.

6. Continue the loop.
- Pick the next best step from the updated state.
- Spawn a new worker when that step should start from a clean context.
- Repeat until the mission is done or truly blocked.

7. Finish with a supervisor summary.
- Report what was completed.
- Report checks run and outcomes.
- Report any remaining blocker, risk, or next action.

## Delegation Heuristics

Prefer a fresh worker when:

- the next step has a clean boundary
- the main thread is accumulating too much tactical detail
- the next step touches a narrow subsystem
- the step can be verified without broad repo-wide archaeology

Prefer staying in the main agent when:

- the next decision depends on nuanced cross-step reasoning
- the task is too small to justify handoff overhead
- integration, prioritization, or tradeoff analysis is the core work

Choose worker type by task shape:

- use a `worker` for scoped implementation or fixes
- use an `explorer` for bounded codebase questions or discovery
- stay in the main agent for planning, integration, sequencing, review of worker output, and git boundary decisions

These are defaults, not rigid rules.
Prefer the lightest coordination model that preserves quality, momentum, and context budget.

## Worker Contract

Each worker assignment should specify:

- scoped objective
- likely files or directories
- constraints and out-of-scope boundaries
- exact verification required
- required response:
  - changes made
  - checks run
  - blockers or follow-ups

Do not ask the worker to own the whole project.
Ask it to finish one bounded assignment cleanly.

## Delivery Policy

Default delivery mode under `$supervisor` is:

- one working branch
- supervisor-owned git decisions
- one PR at the end when the overall goal or milestone is ready

Default ownership split:

- workers implement scoped assignments and run local checks
- workers do not commit, push, open PRs, or merge
- the supervisor reviews worker output, integrates accepted results, and decides when to create checkpoint commits
- the supervisor opens the PR once the work is reviewable as a coherent unit
- the supervisor merges only when the user's requested goal includes merge completion

Use checkpoint commits only when they improve safety or resumability.
Do not create noisy commit history just because a worker completed a step.

Use multiple PRs only when one of these is true:

- the work is intentionally stacked
- slices are independently reviewable and mergeable
- one large PR would materially increase review or rollback risk

Even in stacked work, the supervisor owns PR boundaries.

## Quality Gates

Quality ownership is split by scope:

- workers run the narrow local checks needed for their assignment
- the supervisor evaluates integrated behavior across accepted steps
- the supervisor owns `$slop` and `$review` before shipping decisions

Run quality gates at milestone boundaries, not after every tiny worker step.

Default sequence:

1. worker finishes a scoped assignment and runs step-local checks
2. supervisor reviews and integrates the result
3. once the current milestone is functionally complete, run `$slop` on the integrated diff
4. fix or delegate any cleanup that falls out of `$slop`
5. run `$review` before commit, PR, or merge decisions

Use additional `$review` passes whenever the integrated diff changes materially after review.
Do not require every worker to run full-project cleanup or review unless the task genuinely calls for it.

## Progress Model

At each loop, keep only these facts current:

- `Goal`
- `Plan`
- `Current status`
- `Completed work`
- `Checks`
- `Open blockers or risks`
- `Next step`

If the user wants a reusable continuation seed for a fresh top-level context, use `$relay`.

If the task is long-running or likely to pressure context limits, keep this state in one concise progress artifact.
Prefer a compact record with exactly:

- `Goal`
- `Done`
- `Next`
- `Checks`
- `Risks`
- `Delivery state`

Update it at meaningful stage boundaries so the supervisor can recover cleanly from context loss.
Do not turn the artifact into a verbose diary.

Before the main thread gets overloaded, compress durable state into this artifact or `$relay`.
Do not rely on conversational memory alone for long-running supervision.

## Output Contract

When using `$supervisor`, report progress as:

1. `Goal`
- Current objective and definition of done.

2. `Plan`
- Ordered steps or current execution track.

3. `Latest result`
- What the last worker or local step accomplished.

4. `Checks`
- Exact verification run and outcomes.

5. `Status`
- `continue`, `blocked`, or `done`

6. `Summary`
- Concise rollup of completed work and what remains, if anything.

7. `Delivery state`
- local only, ready to commit, ready for PR, PR open, or merged

## Guardrails

- Do not stop after making a plan if execution is still justified.
- Do not keep one worker alive across many steps just because it already has context.
- Do not pass full conversation history into each worker by default.
- Do not delegate work that is immediately blocking the next local decision when the parent should handle it directly.
- Do not confuse delegation with abdication; the supervisor remains responsible for review, integration, and direction.
- Do not let context-management overhead dominate the task. Small work can stay local.
- Do not treat "auto-merge enabled" or other deferred actions as completed outcomes unless they are actually complete.
- Do not let workers invent git or PR boundaries; those belong to the supervisor.
- Do not open a PR per worker step by default; prefer one coherent PR at the end.
- Do not rely on memory alone for long-running work; keep a compact durable progress record when the task has many steps.
- Do not make the workflow more prescriptive than the task requires; use judgment and adapt the level of structure to the actual risk, ambiguity, and context pressure.
