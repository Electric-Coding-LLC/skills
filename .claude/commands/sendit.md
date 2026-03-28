Stage, commit, push, open a PR, wait for checks, merge, and clean up — all in one uninterrupted flow. Treat invocation of this command as authorization to open the PR and continue through actual merge completion once the merge gates are satisfied.

## Prerequisites

- Require GitHub CLI `gh`. Run `gh --version` and stop if missing.
- Require authenticated `gh` session. Run `gh auth status` and stop if not authenticated.
- Resolve the repository default branch:
  - `gh repo view --json defaultBranchRef -q '.defaultBranchRef.name'`

## Naming Conventions

- Branch: `claude/{description}` when starting from main/master/default.
- Commit: `{description}` (terse).
- PR title: `[claude] {description}` summarizing the full diff.

## Workflow

1. Prepare and open the PR.
- If on main/master/default, create a branch: `git checkout -b "claude/{description}"`
- Otherwise stay on the current branch.
- Confirm status, then stage everything:
  - `git status -sb`
  - `git add -A`
- Commit tersely with the description:
  - `git commit -m "{description}"`
- Run checks if they have not already been run. If checks fail due to missing deps/tools, install dependencies and rerun once.
- Push with tracking:
  - `git push -u origin $(git branch --show-current)`
- If push fails due to workflow auth errors, pull from the default branch and retry the push.
- Open a draft PR:
  - `GH_PROMPT_DISABLED=1 GIT_TERMINAL_PROMPT=0 gh pr create --draft --fill --head $(git branch --show-current)`
- Write the PR description to a temp file with real newlines and update the PR body from that file.
- PR description must cover the issue, user impact, root cause, fix, and validation performed.

2. Resolve the target PR state.
- Use the current branch PR unless the user explicitly names a different PR:
  - `gh pr view --json number,state,isDraft,headRefName,url`
- Stop if the PR is already merged or closed.
- If the PR is draft, mark it ready for review before continuing:
  - `gh pr ready <pr-number>`
- Stop and report if marking it ready fails.

3. Verify merge readiness.
- Watch checks:
  - `gh pr checks <pr-number> --watch --fail-fast`
- Inspect readiness:
  - `gh pr view <pr-number> --json reviewDecision,mergeable,mergeStateStatus,statusCheckRollup,url`
- Stop and report if required checks fail or merge is blocked.
- If checks are still running and the PR is otherwise mergeable, enable auto-merge:
  - `gh pr merge <pr-number> --auto --squash --delete-branch`
- Keep watching until the PR is actually merged. Auto-merge enabled is not completion.

4. Merge and clean up.
- Default merge strategy to squash unless the user requests otherwise:
  - `gh pr merge <pr-number> --squash --delete-branch`
- Ensure you are not on the PR branch before local cleanup.
- Switch to the default branch and fast-forward:
  - `git switch <default-branch>`
  - `git pull --ff-only`
- Delete the local head branch if it still exists:
  - `git branch -d <head-branch>`
- Prune stale remote refs:
  - `git remote prune origin`

## Guardrails

- Never merge if required checks are failing unless the user explicitly requests that override.
- Never treat "auto-merge enabled" as successful completion.
- Never use destructive git recovery commands (`git reset --hard`, `git checkout --`) as part of this flow.
- If uncommitted local changes block checkout or cleanup, stop and ask before stashing or changing branches.
- If `gh pr merge --delete-branch` already removed the remote branch, treat that as success and continue local cleanup.
- If the user explicitly says to keep the PR in draft, stop after PR creation instead of marking it ready.

## Output Contract

- Report:
  - Branch name, commit, PR number, and PR URL.
  - Whether the PR had to be marked ready for review.
  - Check outcome, including notable failing check names if blocked.
  - Merge strategy used.
  - Whether auto-merge was enabled as an intermediate step.
  - Confirmed merged state.
  - Cleanup actions performed.
- If blocked, report exactly which step failed and the next action needed from the user.
