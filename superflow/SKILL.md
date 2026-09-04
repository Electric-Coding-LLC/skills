---
name: superflow
description: Complete one active EXECMAP, roadmap version, or project delivery unit through planning, implementation, review, and GitHub merge. Use for coordinated execution of a larger goal; public release and publish scripts remain separate unless requested.
---

# Superflow

Own one delivery unit from a clear goal to a merged PR. An explicit `$superflow` request authorizes that GitHub delivery unless the user narrows the finish line. Recognize automatic deployment triggered by merge, but leave separate release, deploy, and store-submission commands outside scope unless authorized.

## Scope and completion

Read the active plan, roadmap, repository instructions, current changes, and verification path before reframing the work. An existing `EXECMAP.md` is the default delivery unit: complete its executable work rather than shrinking to the next checkbox. Follow a narrower user request or split a map only when it clearly spans independent deliveries.

Completion means the mapped implementation and exit criteria are true, required review and verification pass, planning state is accurate, and the intended PR is actually merged. Missing prerequisites or unresolved product decisions may block execution; routine engineering choices should not.

## Workflow

1. Establish or update the map with `$execmap` before substantial implementation.
- Promote one selected roadmap version into the repository's active execution artifact when applicable.
- Follow the repo's planning layout; keep step state in the execution map and version state in the roadmap.
- Add step documents only when they clarify work that the map cannot express concisely.

2. Use `$supervisor` for execution and integration.
- Delegate bounded work when independent assignments improve focus or context use; keep tightly coupled work local.
- Give workers the relevant scope, constraints, files, and checks. The supervisor owns integration, git, and PR decisions.
- Reuse the active map for durable progress. Do not create a parallel tracker or impose another fixed field schema.

3. Resolve UI direction when needed.
- For ambiguous or high-impact visual work, use `$design` before implementation and match the existing design system.
- Use `$wirefmt` to format or validate ASCII wireframes only when they materially clarify layout, interaction, or states.
- Keep durable design decisions with the relevant plan; narrow repairs with clear existing patterns do not need a separate design exercise.

4. Complete the mapped work in order.
- Update the map when evidence changes sequence or scope.
- Run targeted checks during implementation and keep factual progress current at meaningful milestones.
- Continue until the delivery unit is complete or truly blocked, without absorbing later roadmap versions.

5. Run `$review` on the integrated intended change.
- Resolve blocking slop, correctness, security, style, and verification findings.
- Reuse current review evidence from the supervisor; rerun affected checks or review after material changes rather than repeating an unchanged gate.
- An unavailable required local check is a blocker unless the repo documents that no local equivalent exists.

6. Run `$sendit` for GitHub delivery.
- `$sendit` owns the final pre-staging reconciliation of existing planning artifacts, commit, push, PR, merge, and cleanup.
- With `$execmap` semantics, implementation completion and closing the active plan can land with the code once exit criteria and verification are true. Do not claim merge or public release before it occurs.
- Wait for the real merge result. Report that result rather than editing repo docs afterward solely to record it.

## Reporting

Give concise milestone updates: result, relevant checks, and the next dependency or blocker. On completion, report the delivered scope, verification, PR/merge result, cleanup, and any separately authorized or still-manual release work. Nested skills need not repeat their full output templates.
