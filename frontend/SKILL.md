---
name: frontend
description: Rapidly implement and refine existing frontend UI in an active feedback loop while deferring comprehensive verification. Use when the user invokes `$frontend` or explicitly asks for quick frontend iteration on layout, styling, copy, responsive behavior, accessibility, or local interactions without running full lint, typecheck, build, test, or Playwright suites between passes. Do not use for release readiness, comprehensive verification, or backend, data, authentication, or API work.
---

# Frontend

## Goal

Iterate on an existing interface with the shortest reliable feedback loop.
Preserve the quality bar while deferring expensive proof until the user asks for verification, review, or delivery.

Treat this skill as an iteration mode, not authorization to ship and not evidence of release readiness.

## Workflow

1. Define the current pass.
- Inspect the user's request, supplied screenshot, target viewport, nearby code, and rendered state when available.
- Infer reversible visual details from context and state material assumptions briefly.
- Scope one coherent layout, styling, copy, accessibility, responsive, or local-interaction pass.

2. Establish the fast loop.
- Reuse an existing dev server, authenticated browser state, and hot reload when they are already available.
- Avoid restarting working infrastructure or starting a production-mode server solely for iteration.
- Match the user's viewport and platform when visual geometry matters.
- Avoid booting a separate browser-test harness unless the user explicitly requests it.

3. Implement the pass.
- Make the smallest coherent change at the correct source boundary.
- Follow existing components, tokens, conventions, and dependency direction.
- Preserve requested copy and unaffected behavior exactly.
- Keep accessibility semantics, keyboard behavior, focus behavior, and contrast intact.
- Avoid new dependencies, speculative abstractions, unrelated cleanup, and broad refactors.
- Avoid adding or rewriting tests while visual details are still moving unless the user requests a specific regression test.

4. Gather lightweight feedback.
- Inspect the rendered result directly when a usable app session is available.
- Compare the property actually being changed: hierarchy, spacing, dimensions, typography, color, overflow, focus, or interaction.
- Run a targeted check only when it is quick and directly catches a likely mistake in the touched path.
- Prefer one changed-file check, one focused existing test, or one narrow compile check over a command chain.
- Stop after the current claim has enough evidence for another iteration; do not expand into release proof.

5. Report and continue.
- State what changed and what was observed in the active UI.
- Name any lightweight check that ran.
- State comprehensive checks as deferred, not passed.
- Continue from user feedback without re-running unchanged checks or re-establishing working infrastructure.

6. Exit the iteration loop.
- When the user accepts the UI, summarize the changed files and deferred verification.
- Do not automatically invoke `$verify`, `$review`, `$sendit`, or `$flow`.
- If the user explicitly asks to verify, review, or ship, hand off to the requested workflow and follow its stricter gates.

## Expensive Work Deferred by Default

Do not run these during `$frontend` unless the user explicitly requests the specific check:

- full-repository lint or formatting suites
- full-repository typechecking
- production builds or integrated `check` commands
- complete unit, integration, or end-to-end suites
- Playwright, Cypress, or other automated browser suites
- production-mode test servers
- multi-browser or multi-viewport matrices
- screenshot snapshot regeneration
- broad accessibility, performance, or bundle audits

Never weaken, delete, skip in CI, or reconfigure required checks to make local iteration faster. Defer running them; do not change their integrity.

## Scope Boundaries

Keep working without pausing for ordinary reversible frontend decisions supported by the current design and codebase.

Stop and surface the boundary when the requested pass requires:

- backend behavior or API contract changes
- data models, migrations, persistence, or destructive state changes
- authentication, authorization, or permission changes
- a shared architectural change outside the active frontend surface
- a bug whose cause is not yet understood and should move to `$debug`

Do not broaden `$frontend` to absorb those changes silently.

## Reporting Contract

During iteration, report only:

1. `Changed` — the coherent UI pass just completed.
2. `Observed` — direct rendered evidence, if available.
3. `Deferred` — broad checks intentionally not run.

At the end, distinguish clearly among source inspection, rendered inspection, targeted checks, and comprehensive verification. Never describe a `$frontend` pass as fully verified or ready to ship unless a separate workflow has supplied that proof.
