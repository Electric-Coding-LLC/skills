---
name: surgery
description: Explicit shorthand for the debug repair workflow when the user asks for surgery. Preserve surrounding behavior and architecture while fixing the root cause.
---

# Surgery

Use this shorthand only when the user explicitly requests `$surgery` or surgery on the code.

Read and follow [Debug](../debug/SKILL.md). It owns reproduction, root-cause analysis, the smallest cohesive repair, and verification of the original symptom. Emphasize the user's requested repair boundary and preserve surrounding design patterns.

This shorthand adds no separate process, delivery authorization, or dependency approval requirement. For ordinary bug reports, use `$debug` directly. For feature implementation or architecture work, use the workflow appropriate to that task.
