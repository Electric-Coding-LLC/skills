---
name: supervisor
description: Lead a larger task with bounded delegation, integrated review, and durable progress. Use when the user wants supervised execution with subagents or when a requested orchestration workflow calls for it.
---

# Supervisor

Own the user's objective, integration, and next decision. Delegate when it improves execution; do tightly coupled work locally. Supervision does not itself authorize commits, pushes, PRs, merges, or publication. Preserve the user's scope and any authorization supplied by the parent workflow.

## Execution loop

1. Establish the mission.
- Identify the outcome, constraints, non-goals, and practical definition of done.
- Read the repository's real state and any existing plan before choosing work.
- Use an existing map or checklist when present; otherwise keep a short ordered plan appropriate to the task.

2. Choose local work or delegation.
- Delegate a bounded assignment when it can proceed independently alongside useful local work and reduces context load or improves focus.
- Keep immediate dependencies, integration, prioritization, and decisions requiring cross-step context in the main agent.
- Use the agent tools available in the environment rather than assuming named worker types exist.
- Prefer a fresh worker for an independent assignment; reuse a worker when a follow-up benefits from its narrow context.

3. Brief the worker.
- Provide the objective, relevant files, constraints, necessary context, checks, and required outcome.
- Assign clear edit ownership. Workers must preserve others' changes and avoid concurrent edits to the same surface unless explicitly coordinated.
- Workers report changes, evidence, and remaining gaps; they do not own git or PR boundaries.
- Pass only the necessary context, including accepted scope and permissions, rather than a full transcript by default.

4. Review and integrate.
- Inspect the actual result and evidence, not just the worker's completion claim.
- Accept, repair, or redirect the work and verify interactions between accepted changes.
- Replan from new evidence while preserving the user's objective.
- Run cleanup-aware `$review` on the integrated milestone before an authorized shipping decision. Reuse still-valid checks; repeat affected review when the diff changes materially.

5. Continue or finish.
- Choose the next dependency from current state and continue until the requested outcome is complete or genuinely blocked.
- Ask for input only when unresolved product intent, a material tradeoff, or an unavailable prerequisite prevents safe progress.
- Do not end at planning or delegation when execution remains authorized and feasible.

## Progress and delivery ownership

Keep durable state in the existing plan or execution map when work is long enough to risk context loss. Record completed work, the next dependency, verification, and relevant risks without creating a parallel tracker or requiring a fixed schema. Use `$relay` when the user requests a continuation seed.

The main agent owns integration and all authorized git actions. Prefer one coherent PR per delivery unit; use multiple PRs only for independently reviewable slices or a justified stacked workflow. Checkpoint commits require existing commit authorization and should serve resumability rather than record every worker step.

When the requested goal includes delivery, use the appropriate authorized workflow: `$sendit` for GitHub completion or `$deliver` for a ready change through production verification. An open PR or enabled auto-merge is not a completed merge.

## Reporting

Keep milestone updates concise: what changed, what was proved, and what happens next. Finish with the outcome, verification, actual delivery state if applicable, and material remaining risks or blockers. Avoid repeating nested skill output templates.
