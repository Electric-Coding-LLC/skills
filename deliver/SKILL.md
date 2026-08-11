---
name: deliver
description: Review a ready code change, resolve actionable findings, open and merge its GitHub pull request, deploy it through the repository's real release path, and verify the live result. Use when the user asks to deliver, ship, release, deploy, or take a completed change through review, merge, publication, and production proof in one flow.
---

# Deliver

## Goal

Take an implementation that is ready for a delivery gate from local changes to verified production. Treat `$deliver` as authorization for review, necessary fixes, GitHub merge, deployment, and live verification.

Do not use this skill to plan or implement a new feature. Use `$superflow` for a larger roadmap delivery unit.

## Delivery contract discovery

Before changing or publishing anything, determine the repository's actual delivery contract. Inspect, in order:

1. `AGENTS.md`, release/deployment docs, active plan or `EXECMAP.md`.
2. Repository scripts, CI workflows, hosting configuration, and recent merged-release history.
3. Existing project-specific skills or known runbooks.

Record the chosen contract briefly: merge trigger, deployment command or workflow, target environment, required checks, health/version/live proof, local dev-server cleanup command when present, and rollback or stop condition.

Use the discovered contract; do not assume every repository deploys on merge.

If the contract is not documented, infer it only from strong repository evidence. If production target, publish mechanism, or verification boundary remains materially ambiguous, stop before publish and ask one concise question. After it is answered, add a short repo-local delivery note only when the project has a natural existing place for it.

## Workflow

1. Establish scope and readiness.
- Inspect working tree, intended diff, branch, and current project state.
- Preserve unrelated user changes. Do not stage or alter them.
- Confirm the implementation is complete enough for a release gate; otherwise state the missing work.

2. Run `$review`.
- Run the complete review gate on the integrated diff, including repo-native checks.
- Resolve blocking findings and any clearly worthwhile non-blocking findings within the request's scope.
- Re-run focused checks and `$review` until the verdict is ready, or stop with a concrete blocker.
- Do not weaken checks or silently accept unresolved production risk.

3. Run `$sendit`.
- Use `$sendit` to stage only intended changes, commit, push, open the pull request, wait for required checks, merge safely, and clean up.
- Treat confirmed merge, not an open PR or enabled auto-merge, as the GitHub completion boundary.

4. Deploy using the selected contract.
- For merge-triggered CI/CD: identify the workflow caused by the merged revision, wait for its terminal success, and confirm the deployed revision when the platform exposes it.
- For OpenAI Sites: publish the exact merged revision using the Sites hosting workflow; wait for successful publication and inspect the authenticated production application.
- For another explicit provider or release script: use its documented production command and wait for the real terminal deployment state.
- Never publish a different revision than the merged one unless the repository's release process explicitly requires it and the discrepancy is explained.

5. Verify production.
- Run the repository's strongest cheap live proof: health endpoint, version/revision endpoint, targeted smoke path, asset/PWA verification, or authenticated production check as appropriate.
- Verify the user-visible behavior affected by the change when feasible; deployment success alone is not proof.
- If live verification fails, report the exact boundary that failed and do not claim delivery completion.

6. Release local runtime resources.
- After production verification, stop dev servers, watchers, and related runtime processes started by the delivery task or confirmed to be running from the delivery worktree.
- Prefer the repository's documented cleanup command or configured Codex Local Environment cleanup script when it performs this shutdown.
- Identify ownership from the recorded process handle, process working directory, command, and listening port. Terminate gracefully first; use force only when the exact process is confirmed and does not exit after a brief wait.
- Do not stop a server that predated the task or belongs to another checkout, worktree, project, or user session.
- Verify that every affected port is no longer listening. If no owned runtime process exists, record `not applicable`.
- If cleanup fails, preserve unrelated processes, report the exact process and port still active, and leave delivery marked incomplete until cleanup succeeds or the user explicitly accepts the remaining cleanup.

## Guardrails

- Stop before external publication when deployment authority, target, or release contract is unclear.
- Do not treat a successful build, merged PR, queued job, or enabled auto-merge as production delivery.
- Do not invent generic deploy commands when project evidence is absent.
- Do not remove the current worktree directly or kill processes based only on a port number.
- Keep production credentials and secret values out of output.
- Do not run rollback, destructive cleanup, migrations with irreversible effects, or a release to a broader environment without explicit repository guidance or user approval.
- When deployment is intentionally automatic after merge, wait for and verify that path instead of publishing a duplicate release.

## Completion contract

Report, concisely:

1. Review result and fixes made.
2. GitHub delivery result: merged or blocked.
3. Deployment path used and terminal state.
4. Production verification performed and outcome.
5. Local runtime cleanup performed and ports released, or `not applicable`.
6. Remaining risk or required manual follow-up, if any.

Call the work delivered only when review is clean, the intended change is merged, the correct production path has succeeded, a live verification boundary has passed, and runtime processes owned by the delivery task or worktree have been stopped or none exist.
