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
- If PR is draft, ask whether to mark ready before continuing.

2. Verify checks and merge readiness.
- Watch checks:
  - `gh pr checks <pr-number> --watch --fail-fast`
- Inspect merge readiness:
  - `gh pr view <pr-number> --json reviewDecision,mergeable,mergeStateStatus,statusCheckRollup,url`
- Stop and report if required checks fail or merge is blocked.

3. Merge.
- Default merge strategy to squash unless user requests otherwise:
  - `gh pr merge <pr-number> --squash --delete-branch`
- If checks are still running but PR is otherwise mergeable, offer auto-merge:
  - `gh pr merge <pr-number> --auto --squash --delete-branch`

4. Clean up local git state.
- Ensure you are not on the PR branch before deleting it.
- Switch to default branch and fast-forward:
  - `git checkout <default-branch>`
  - `git pull --ff-only`
- Delete local head branch if it still exists:
  - `git branch -d <head-branch>`
- Prune stale remote refs:
  - `git remote prune origin`

## Guardrails

- Never merge if required checks are failing unless the user explicitly requests that override.
- Never use destructive git recovery commands (`git reset --hard`, `git checkout --`) as part of cleanup.
- If uncommitted local changes block checkout/cleanup, stop and ask before stashing or changing branches.
- If `gh pr merge --delete-branch` already removed the remote branch, treat that as success and continue local cleanup.

## Output Contract

- Report:
  - PR number and URL.
  - Check outcome (pass/fail and notable failing check names).
  - Merge strategy used.
  - Cleanup actions performed (local branch deleted, default branch updated, prune run).
- If blocked, report exactly which step failed and the next action needed from the user.
