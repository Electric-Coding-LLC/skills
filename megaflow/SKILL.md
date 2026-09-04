---
name: megaflow
description: Execute a roadmap one version at a time through superflow, reassessing repository state after each merge. Use when the user authorizes repeated version delivery rather than a single milestone; separate publication remains outside scope unless requested.
---

# Megaflow

Own the roadmap loop; `$superflow` owns each version's implementation and GitHub delivery. Select one version at a time and re-evaluate whether the next version remains justified after each merge.

## Workflow

1. Read the roadmap, active plan, repository instructions, and shipped-state evidence.
- Identify version order, the current active unit, completion semantics, and the user's finish line.
- Distinguish internal implementation completion, GitHub merge, and public release.
- Resolve material conflicts before acting. Correct obvious stale implementation status from evidence rather than treating every discrepancy as a user decision.

2. Select and promote one version.
- Prefer an already active incomplete unit; otherwise choose the next justified roadmap version.
- Promote it through the repository's planning convention or `$execmap` when needed.
- Keep step state in the execution map and version state in the roadmap. Do not create a second progress tracker.

3. Run `$superflow` for that version.
- Use a fresh subagent or execution context when available and useful, passing only the version, relevant evidence, constraints, and existing authorization.
- Let `$superflow` own planning, optional design, implementation, review, and GitHub delivery for this unit.
- If it returns `local only`, `ready for PR`, `PR open`, or `auto-merge enabled`, continue the same unit until it is actually merged or truly blocked.
- Do not delegate later versions concurrently.

4. Check completion and planning state.
- Before the version's delivery commit, `$sendit` reconciles implementation and verification status in the existing plan and roadmap. With `$execmap` semantics, mark internal completion and close the active plan when its exit criteria are true, so those updates land with the code.
- After merge, re-read the roadmap and plan and verify the merge result. This is a consistency check, not a mandatory post-merge editing phase.
- Do not make merge-only status a required repo-doc update or create a follow-up docs-only PR merely to record the merge.
- If the repo explicitly requires a different post-merge tracker transition, follow its documented mechanism within the authorized scope. Do not silently redefine its statuses.
- Carry newly discovered, mechanically obvious factual corrections into the next justified delivery's planning changes. If there is no next delivery, report any remaining tracker discrepancy without manufacturing another version or PR.

5. Continue only when justified.
- The current version must be merged and the roadmap must identify a clear next unit that still makes sense after what landed.
- Stop when the authorized roadmap is complete, scope or priority needs a user decision, or access and other prerequisites block safe work.
- Manual public release remaining outside scope does not prevent internal roadmap completion.

## Reporting

At loop boundaries, report the current version, actual delivery result, relevant verification, and why execution continues or stops. Use the roadmap and active map for durable state; preserve the next dependency and any blocker across context changes. Do not emit a second fixed progress schema for the outer loop.
