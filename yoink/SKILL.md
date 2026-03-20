---
name: yoink
description: "Finalize a GitHub pull request after it is opened (often after using Yeet): verify CI/check status, merge safely with GitHub CLI, and clean up local/remote branches. Use when the user asks to merge a PR, wait for checks, or perform post-merge cleanup."
---

# Yoink

## Prerequisites

- Require GitHub CLI `gh`. Run `gh --version` and stop if missing.
- Require authenticated `gh` session. Run `gh auth status` and stop if not authenticated.
- Resolve the repository default branch:
  - `gh repo view --json defaultBranchRef -q '.defaultBranchRef.name'`
- Resolve the target PR:
  - Prefer user-provided PR number.
  - Otherwise use the current branch PR with:
    - `gh pr view --json number,state,isDraft,headRefName,url`

## Workflow

1. Validate PR state.
- Stop if the PR is already merged or closed.
- If PR is draft, mark it ready for review before continuing:
  - `gh pr ready <pr-number>`
- Stop and report if marking the PR ready fails.

2. Verify checks and merge readiness.
- Watch checks:
  - `gh pr checks <pr-number> --watch --fail-fast`
- Inspect merge readiness:
  - `gh pr view <pr-number> --json reviewDecision,mergeable,mergeStateStatus,statusCheckRollup,url`
- Stop and report if required checks fail or merge is blocked.
- If checks are still running, continue waiting and polling until they resolve unless an external blocker requires user input.
- Auto-merge may be enabled when policy allows, but that is only an intermediate step. `$yoink` is not complete until the PR is actually merged.

3. Merge.
- Default merge strategy to squash unless user requests otherwise:
  - `gh pr merge <pr-number> --squash --delete-branch`
- If checks are still running but PR is otherwise mergeable, offer auto-merge:
  - `gh pr merge <pr-number> --auto --squash --delete-branch`
- After enabling auto-merge, keep watching until the PR is actually merged.

4. Clean up local git state.
- Ensure you are not on the PR branch before deleting it.
- Switch to default branch and fast-forward:
  - `git switch <default-branch>`
  - `git pull --ff-only`
- Delete local head branch if it still exists:
  - `git branch -d <head-branch>`
- Prune stale remote refs:
  - `git remote prune origin`

## Guardrails

- Never merge if required checks are failing unless the user explicitly requests that override.
- Never treat "auto-merge enabled" as a successful completion state.
- Never use destructive git recovery commands (`git reset --hard`, `git checkout --`) as part of cleanup.
- If uncommitted local changes block checkout/cleanup, stop and ask before stashing or changing branches.
- If `gh pr merge --delete-branch` already removed the remote branch, treat that as success and continue local cleanup.
- If the user explicitly says to keep the PR in draft, stop instead of marking it ready.

## Output Contract

- Report:
  - PR number and URL.
  - Whether the PR had to be auto-marked ready for review.
  - Check outcome (pass/fail and notable failing check names).
  - Merge strategy used.
  - Whether auto-merge was enabled as an intermediate step.
  - Confirmed merged state.
  - Cleanup actions performed (local branch deleted, default branch updated, prune run).
- If blocked, report exactly which step failed and the next action needed from the user.
