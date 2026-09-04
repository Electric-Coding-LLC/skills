---
name: yeet
description: Stage intended changes, commit, push, and open or update a GitHub PR without merging. Use when the user explicitly requests yeet or PR creation only; use sendit for the full merge cycle.
---

# Yeet

Complete PR preparation and leave the PR open. An explicit request authorizes the intended commit, push, and PR; it does not authorize merge or production publication.

## Workflow

1. Read [Sendit](../sendit/SKILL.md) for its prerequisites, scope inspection, final planning sync, and PR preparation steps.
2. Perform only those preparation steps, preserving unrelated committed, staged, unstaged, and untracked work. Reuse an existing intended branch or PR where appropriate.
3. Stop after creating or updating the PR. Leave a new PR in draft unless the user asks to mark it ready; preserve an existing PR's draft/ready status unless requested otherwise.

This PR-only boundary overrides Sendit's full-cycle default. Do not invoke its finalization step or `$yoink` unless the user separately authorizes merge. If the change is already committed, continue to push/PR creation rather than treating an empty working tree as a blocker.

Report the commit, PR URL, draft/ready state, and relevant validation. Describe missing access or other blockers precisely; do not pull another branch to resolve authentication errors.
