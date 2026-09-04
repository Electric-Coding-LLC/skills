---
name: sendit
description: Stage the intended changes, commit, push, open or update a GitHub PR, wait for checks, merge, and clean up. Use when the user requests the full GitHub delivery cycle; preserve any narrower finish line such as draft-only or PR-only.
---

# Sendit

Own GitHub delivery of a ready change. An explicit `$sendit` request authorizes the full PR and merge cycle unless the user limits it. A parent workflow may supply that authorization; merely selecting this skill does not grant it. Production publication is a separate scope, though merge may trigger the repository's automatic deployment.

## Prerequisites and scope

- Check `gh --version` and `gh auth status`. Use an existing configured credential path when available; never print secrets. If access is still unavailable, report the exact blocker.
- Resolve the repository and default branch with `gh repo view --json defaultBranchRef -q '.defaultBranchRef.name'`.
- Inspect the current branch, intended base, complete branch diff, and local work before staging: `git status -sb`, `git diff`, `git diff --cached`, and relevant untracked files. Do not include unrelated committed changes just because they share the branch.
- Reuse an existing PR for the intended branch. If the intended delivery is already merged and no new intended work remains, use `$yoink` only for remaining authorized cleanup. If the user is delivering new work on a previously merged branch, isolate that new change on an appropriate branch rather than reusing the old PR. For a closed unmerged PR, resolve whether the current request calls for reopening or a new delivery before proceeding.
- On the default branch, create `codex/{description}` for the intended change. Otherwise use the current branch only when it belongs to this work. Preserve other branches and worktrees.

## Final planning sync

Before staging, reconcile existing task artifacts such as `PLAN.md`, `EXECMAP.md`, roadmap entries, and checklists with implementation and verification evidence.

- Update factual completion, verification, and ready-to-land state so it lands with the code. Prefer a repo helper when available and useful.
- Under `$execmap` semantics, roadmap `completed` means internal implementation completion. Mark it and close the active plan before delivery when exit criteria and required checks are true; GitHub remains the source of truth for merge state.
- Follow the repository's actual status meanings. Do not mark merge, deployment, or release criteria complete before they occur. A documented post-merge tracker integration may handle those separately.
- Do not create another tracker or a follow-up docs-only PR just to record merge completion. Ask only when the correct update depends on an unresolved material decision.

## Prepare and open the PR

1. Establish readiness.
- Run required repo-native checks if valid evidence for the current content is absent. Fix change-caused failures and rerun affected checks.
- Restore missing dependencies with the repository's documented tooling when appropriate; do not change dependency versions or lockfiles just to bypass an environment problem.
- Finalize planning status from the results above. Recheck affected evidence if those edits change executable content.

2. Stage and commit only intended changes.
- Inspect any existing staged content. Leaving an unrelated staged file untouched does not exclude it from a commit. Preserve that index state and stop before committing if the intended changes cannot be isolated safely; do not silently unstage or commit someone else's work.
- Stage explicit paths or hunks. Use `git add -A` only when the user requests all changes or inspection proves everything belongs to the task.
- Inspect the staged diff and run `git diff --cached --check` before committing.
- Use a concise commit message. If the intended change is already committed, continue without creating an empty commit.

3. Push the intended branch.
- Use `git push -u origin <branch>`.
- Diagnose a rejection before recovery. Authentication or workflow-permission errors require correcting the authorized credential path, not pulling another branch. Retry after the cause is resolved.
- For branch divergence, inspect local and remote history before integrating anything. Preserve unrelated work, avoid blind pulls or force pushes, and revalidate any changed content before proceeding.

4. Create or update the intended PR.
- Prefer the repository's title and description conventions; summarize the final behavior and validation concisely.
- Write multiline descriptions to a temporary file and pass it with `--body-file` to `gh pr create` or `gh pr edit`. Do not execute the body file or interpolate its prose into shell code.
- If no PR exists, create a draft with an explicit head and verified base, for example `gh pr create --draft --head <branch> --base <base> --title <title> --body-file <file>`.
- Mention relevant planning changes when useful to reviewers; do not require boilerplate declaring that no planning sync was needed.

## Finalize

If the user requested PR-only or draft-only delivery, stop at that finish line and report it accurately. Otherwise follow [Yoink](../yoink/SKILL.md) for draft readiness, checks, merge, and cleanup. Pass the reviewed revision and existing authorization; do not ask for approval again.

Keep owning the result until the PR is actually merged or a concrete blocker requires user action. Enabled auto-merge or a queued merge is not completion.

## Report

Report the change, commit, PR URL, check outcome, actual merge or draft state, and cleanup. Mention planning updates and residual work only when material. Preserve local work if it prevents cleanup, report the remaining boundary, and never use destructive git recovery to force completion.
