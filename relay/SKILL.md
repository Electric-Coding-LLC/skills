---
name: relay
description: Capture the current task state and next action in a compact handoff for a fresh context, preserving the user's scope, constraints, and authorization.
---

# Relay

Produce a short state snapshot that another context can continue without reconstructing the conversation.

## Handoff contents

- Completed work and its relevant verification.
- The immediate next action, including a blocker or measurement step when that is what remains.
- Necessary files, branch or worktree, commands, and existing plan location.
- The user's accepted scope, important constraints, and current workflow.
- Delivery authorization already granted, and any consequential action still awaiting authorization.

Include only facts needed to resume. Distinguish confirmed results from assumptions and unfinished work. Reuse an existing plan as the durable source of truth rather than copying its full contents.

## Workflow and authorization

A handoff preserves permission; it does not create it. Do not turn unfinished implementation into authorization to commit, push, merge, deploy, or broaden the task.

Carry forward a skill only when it was already selected for the continuing work or the user requests it. For example, local `$frontend` iteration stays in that workflow; an authorized `$flow` may retain its merge scope. If no workflow was selected, state the next action without inventing a skill command. If the track is done, say so without manufacturing follow-up work.

## Output

Return a concise `Done` summary, `Next` action, and a fenced `Continue from` seed suitable for pasting into a fresh context. The seed should contain the minimum state, constraints, authorization, and next action required to resume. Avoid transcript recap and repeated workflow narration.
