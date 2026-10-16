---
name: deliver
description: "Take a finished change to its users: review it, fix what the review finds, merge it, take it live per the repo's contract, and verify it live. Use when Chris signals that finished work should ship, in any words."
---

# Deliver

When Chris signals that finished work should ship, the implementation is complete and nothing else belongs in it. That signal is authorization for everything below: commits, the review and its fixes, push, PR, squash auto-merge, CI and review-thread fixes, the repo's go-live steps, live verification, and branch cleanup. **Delivered means verified live**, not merged.

Stop and ask only for a product decision, a change of scope, a go-live step the contract says to ask about, or something irreversible the plan did not name (a package version, a data migration). Never force-push, change repository settings, or merge past a failing check or an unresolved thread.

## 1. Read the contract

- Read the `## Delivery` section of the repo's `AGENTS.md`, and the sections it points to (Code Review Rules, Releasing).
- **No contract:** draft one from repo evidence (package scripts, CI and deploy workflows, docs), using the template at the end of this file. Show it to Chris and wait for approval before going on; commit it with the change.
- `git status -sb`. Know exactly what is being delivered. Leave unrelated changes alone and say so.
- If the contract, or the global rule for solo repos, allows direct commits for this kind of change, commit to the default branch, run the gate and go-live steps, and skip the PR steps below. Otherwise, if on the default branch, create a branch named for the change (`drop-retired-tables`), with no prefix unless the situation warrants one.

## 2. Gate

Run the contract's gate. It must pass before the review.

## 3. Review, sized to the change

The reviewer is a separate agent (a subagent) with a fresh context, or, if the app can't start one, a separate pass that rereads the diff cold: it reviews the diff hunting for failure cases users would hit in the contract's review focus (not confirming the happy path), verifies each finding, and reports severity, file:line, scenario and class.

**Light review** when the change is small (a few files, roughly under 150 changed lines) and touches none of the contract's sensitive areas: one reviewer agent, told to keep it brief.

**Full review** otherwise, one round:
1. One reviewer agent over the whole diff, briefed on what changed and where the risks are.
2. Fix the real findings. Re-review a fix (a reviewer agent on the fix commit) only when it touches security, sessions or data; otherwise the tests that prove the fix are enough. Anything found after that round, or any edge case not worth a fix, goes in the PR's known limitations, not another round. The review loop is where time goes; one thorough round catches what matters.

Prefer the simplest design that is correct; when a fix starts growing machinery (resolvers, observers, state tracking) to cover edge cases, step back to a simpler rule (a full reload, an explicit flag, refusing the case) and decide that yourself; it is engineering judgment, not a product decision. When a round finds only edge cases, stop: list them in the PR instead of running more rounds. Don't turn review edge cases into new follow-up tasks unless they affect real users.

For every real finding: fix it as a class (the same pattern elsewhere, the same rule in other shapes) with one table-driven test; for a bug, confirm the test fails with the fix reverted. For a finding that is not real, note why. A finding outside this change becomes a follow-up task with a self-contained prompt (the app's task tool if it has one, otherwise listed in the report), never part of this delivery.

## 4. Merge

- If the repo uses Changesets and the change touches a published package, add its changeset (`changeset` via the repo's script): the packages and a patch, minor or major bump, with a one-line user-facing summary.
- Commit with a descriptive message (what and why; bullets for several parts) and the session's attribution trailer. Push with `-u`.
- PR description in a temporary file, passed with `--body-file`: **Summary**, **Review** (what ran, what it caught, what was deliberately left alone), **Test plan**, **Follow-ups**, then the attribution line. Not a draft.
- If the app can watch the PR, bind it and turn on Auto-fix. Enable squash auto-merge. If GitHub says the PR is already clean, merge it directly with `gh pr merge <n> --squash`.
- If the app watches the PR and wakes the session, do not poll CI; otherwise wait with `gh pr checks <n> --watch`. Handle what comes back:
  - **CI failure:** read the log and any failure artifact or trace; fix the real cause; re-run the gate; push. Never retry blindly or weaken a test.
  - **Conflict:** merge the default branch in (never rebase or force-push), resolve, verify, push.
  - **Review comment:** verify it; fix real ones as a class; reply with what changed or why not; resolve the thread.
- Layered work: start each layer's branch from the default branch after the previous layer merges. A branch stacked on an unmerged one conflicts once the first is squash-merged.

## 5. Go live and verify

- Where a release is a manual step (a tag, a publish command) rather than a release PR, first confirm every review comment on the pull requests going into it is fixed or answered.
- Follow the contract's go-live steps and deploy policy. For "automatic" where the go-live workflow verifies the live result itself (and GitHub notifies Chris when it fails), the delivery is complete once auto-merge is on and review threads are resolved: report that the workflow verifies live and failures reach him. Never ask Chris to come back and say "check"; confirm the live result yourself the next time you work in that repo. Where nothing verifies automatically, check once when woken. Do not publish a duplicate. For "ask first", stop and report "merged, not delivered", naming what is waiting.
- Verify live using the contract's check: the live version or release endpoint shows the merge commit, the registry shows the new version, or the page shows the change. A successful workflow alone is not proof.
- Clean up: `git switch <default> && git pull --ff-only`; confirm `git diff --quiet <branch> <default>` and `git branch -D <branch>`; delete the remote branch if GitHub did not; `git fetch --prune`. Stop any dev server this work started.

## Report

Short: delivered (verified live, with the evidence) or exactly where it stopped. Then the PR and merge commit, what the review caught, and any follow-ups opened.

Chris is often away while a delivery runs, so end it (and any stop that needs his decision) with a push notification (if the app can send one) leading with the outcome, e.g. "portal 0.3.0: PR #42 auto-merging, Deploy verifies live" or "deliver stopped: review found a sign-in bug, need your call". Never leave him to poll the session.

## Contract template

```markdown
## Delivery

- **Gate:** `<commands that must pass before review and in CI>`
- **While iterating:** `<quick checks for a single package or area>`
- **Review focus:** <the rules a reviewer should hunt against, or a pointer to them>
- **Sensitive areas** (always a full review): <paths or concerns: auth, sessions, data, public API, release tooling>
- **Merge:** squash PR into `<default branch>`; required checks: `<names>`
- **Go live:** <what happens after the merge and what triggers it>
- **Verify live:** <how to prove the change reached users>
- **Deploy policy:** automatic | ask first
```
