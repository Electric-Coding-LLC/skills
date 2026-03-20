---
name: relay
description: Capture what was done and what should happen next, then produce a minimal handoff seed for a fresh context. Use when the user wants to carry progress forward without retyping it.
---

# Relay

## Goal

Carry progress into a fresh context with minimal text.

## Use When

- The user wants a handoff to a fresh Codex context.
- The current context has useful progress that should not be retyped.
- Work should continue in a new session from the current stopping point.

## Instructions

1. State what was done.
- Keep it short.
- Include only the work the next context needs to know.

2. State what is next.
- Name the immediate next step.
- If there is no justified implementation step, say that explicitly.
- Include file paths or commands only when they help.

3. Write a minimal seed for the next context.
- Use plain language only.
- Do not include transcript recap, diffs, or workflow narration.
- Do not write the seed as an instruction to obey.
- Do not use phrasing like `please implement`, `start with`, `use`, or other directive wording.
- Write it as a neutral state snapshot the next context can read and continue from.
- Preserve the real continuation state:
  - If active implementation work is justified, end with `Continue with $flow.`
  - If reassessment or measurement is next, end with `Continue with $chunk.`
  - If the track is done for now, do not add a skill continuation line.
- Only use a different skill line if the user explicitly asks for a different skill.

## Output

Return exactly these three sections:

1. `Done`
- 1 to 5 bullets.
- State completed work only.

2. `Next`
- 1 to 5 bullets.
- State the next action only.
- `No immediate implementation step` is valid.

3. `Continue from`
- A fenced code block the user can paste into a fresh context.
- Restate the same information as compact context, not as a command.
- Prefer labels like `Current state`, `Done`, and `Next`.
- If active work remains, end with `Continue with $flow.` unless the user explicitly asked for a different skill.
- If reassessment or measurement is next, end with `Continue with $chunk.` unless the user explicitly asked for a different skill.
- If the track is done for now, omit the carry-forward skill line.
