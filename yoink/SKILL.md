---
name: yoink
description: Finalize an existing GitHub PR through required checks, merge, and branch cleanup, or finish cleanup for an already-merged PR. Use when the user requests PR finalization; a request only to inspect or wait for checks does not authorize merging.
---

# Yoink

Operate on the user's intended existing PR. Preserve the requested finish line and any authorization inherited from `$sendit` or another delivery workflow. Loading this skill for a status question does not authorize a merge.

## Resolve state

- Check `gh --version`, `gh auth status`, the repository, and its default branch.
- Prefer a supplied PR number or URL; otherwise resolve the current branch PR.
- Inspect state, draft status, head/base branches, and head revision using `gh pr view <pr> --json number,state,isDraft,headRefName,baseRefName,headRefOid,url`.
- If already merged, skip draft, checks, and merge operations and perform only remaining requested cleanup. If closed without merging, report that state; do not reopen it without authorization.
- For an open draft PR, mark it ready with `gh pr ready <pr>` only when review/merge readiness is in scope. Respect a request to keep it draft.

## Checks and merge

1. Verify readiness for the current PR head.
- Inspect `gh pr view <pr> --json reviewDecision,mergeable,mergeStateStatus,statusCheckRollup,headRefOid,url` and repository policy.
- Wait for required checks with `gh pr checks <pr> --required --watch --fail-fast`, and inspect any additional repo-defined gates. Keep the user informed while waiting.
- Distinguish no configured checks from expected checks that have not started. Confirm CI and branch policy before deciding that absent checks are acceptable; report the actual condition rather than inventing a pass.
- Diagnose failures. Fix within the authorized task when appropriate; otherwise report the specific blocker. Recheck the affected content and current head after any fix or new push.

2. Merge only when authorized and gates are satisfied.
- Default to squash unless user or repository policy specifies another method.
- For an ordinary merge, use `gh pr merge <pr> --squash --match-head-commit <verified-sha>`. Handle a head mismatch by inspecting the new change and its checks before retrying.
- Follow a required merge queue instead of bypassing it. Auto-merge may be enabled within existing merge authorization while gates are pending, but continue waiting for the actual result.
- Do not override failing required checks or branch protections without explicit user authority and applicable repository policy.
- Confirm the terminal merged state and merge revision. An open PR, queue entry, or enabled auto-merge is not a merged PR.

## Cleanup

Only clean up after confirming the PR is merged and cleanup is within the request.

- Inspect local state and worktrees before changing branches. Preserve unrelated staged or unstaged work and branches checked out elsewhere; do not stash, reset, or force-delete to complete cleanup.
- In the task-owned checkout, switch to the default branch and update with `git pull --ff-only` when safe. If another worktree owns it, use the repository's worktree cleanup procedure instead of forcing a checkout.
- Delete the merged head branch only after confirming the local/remote ref still belongs to the completed PR and contains no additional work. Use `git branch -d <head>` for safe local deletion; if it refuses, inspect the reason rather than escalating automatically.
- Remove the corresponding remote branch when safe and still present, then prune stale remote refs. An already-deleted branch is success. Never remove a default/base branch or a reused branch merely because its name appeared on the PR.
- Do not remove the current worktree directly. If local work prevents safe cleanup, report the merged result and precise remaining cleanup separately.

## Report

Report the PR URL, checks and actual state, merge revision when applicable, cleanup completed, and anything still blocked. A status-only request ends with status; a cleanup-only request does not trigger another merge.
