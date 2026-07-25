---
name: architect
description: "Use for repository architecture work: establishing or evaluating module boundaries, dependency direction, ownership, file organization, architectural drift, design fit for proposed changes, and concise AGENTS.md repo guidance. Use when the user asks for architecture review, architecture planning, architectural drift, where code should live, or AGENTS.md guidance updates."
---

# Architect

## Goal

Establish, audit, and document the system design for a repository or subsystem.

This skill is for architecture judgment. It should preserve the user's goal while checking whether the proposed design fits the real repository.

Architecture work is standalone. It may lead to a plan, guidance, a general implementation, a cleanup, a refactor, or no code change at all. It does not imply `$surgery` as the implementation companion.

## Operating Rules

- Inspect the current repository before recommending structure.
- Treat active `AGENTS.md` files, existing code, tests, package boundaries, and deployment constraints as source-of-truth evidence.
- Prefer the smallest design-consistent correction over a broad redesign.
- Tie recommendations to concrete files, imports, commands, or repeated patterns.
- Edit guidance docs only when the user asks for changes or the update is part of the requested architecture work.
- Use another skill after architecture work only when that skill is independently triggered by the user's request; use `$surgery` only for explicit surgery requests or repair-oriented bug/hotfix work.

## Workflow

1. Define the architecture question.
- Identify whether the task is about new architecture, drift review, file placement, dependency direction, AGENTS.md guidance, or evaluating a proposed change.
- Separate the user's goal from any suggested mechanism.

2. Discover the real structure.
- Find repo instructions and source-of-truth docs: `AGENTS.md`, README, plans, package manifests, app/service folders, CI config, and test layout.
- Map major modules, packages, apps, routes, services, persistence boundaries, public APIs, and external integrations.
- Check dependency direction with imports, package references, route boundaries, or build configuration when relevant.
- Identify local conventions for naming, file size, tests, validation, errors, and configuration.

3. Evaluate fit.
- Ask whether the proposed or existing structure preserves:
  - clear ownership,
  - cohesive files and modules,
  - dependency direction,
  - real boundaries at APIs, persistence, integrations, packages, or deployment units,
  - testability,
  - performance-sensitive paths,
  - security and reliability boundaries.

4. Recommend the smallest correction.
- Prefer moving or consolidating code only when ownership is genuinely wrong.
- Prefer direct code in the existing layer over one-use contracts, factories, adapters, mappers, registries, or service layers.
- Recommend new boundaries only when there is a real boundary or repeated pressure that the current structure cannot absorb.
- Stage broad repairs into small, reviewable steps.

5. Document durable guidance when useful.
- For `AGENTS.md`, add only repo-specific rules that help future agents avoid recurring mistakes.
- Keep guidance concise, enforceable, and tied to the current codebase.
- Do not copy generic engineering principles that already belong in global instructions.

## Architecture Drift Review

Use this mode when the user asks whether the repo is on track or drifting.

Report:

1. Intended architecture, inferred from docs and code.
2. Actual architecture, based on current files and dependencies.
3. Drift, if any, with file-level evidence.
4. Ownership or boundary ambiguities.
5. Unnecessary abstractions or missing boundaries.
6. Test organization problems.
7. Performance, security, or reliability risks caused by structure.
8. Minimal staged corrections.
9. Exact `AGENTS.md` additions, removals, or edits if guidance is stale.

Do not recommend a large rewrite by default. Prefer immediate guardrails, small cleanup PRs, boundary repairs, then larger refactors only when evidence justifies them.

## New Architecture Planning

Use this mode when establishing architecture for a new project, subsystem, or major feature area.

First identify constraints:

- product goal,
- primary users,
- runtime and framework,
- deployment model,
- team and maintenance expectations,
- data ownership,
- external integrations,
- security and reliability requirements,
- performance-sensitive paths,
- likely near-term changes.

Then propose:

- architecture style in plain language,
- directory structure,
- module and package boundaries,
- dependency rules,
- persistence and API ownership,
- error-handling and configuration model,
- testing strategy,
- observability, performance, and security guardrails,
- AGENTS.md guidance worth persisting.

Reject heavier patterns such as microservices, event systems, plugin systems, clean architecture, DDD, CQRS, dependency injection, or broad contract layers unless the constraints clearly justify them.

## AGENTS.md Guidance

When creating or updating repo guidance, include only sections that are useful for that repository. A practical `AGENTS.md` can include:

- project map,
- commands,
- architecture boundaries,
- dependency direction,
- file placement rules,
- testing conventions,
- performance-sensitive areas,
- security-sensitive areas,
- explicit do-not rules,
- definition of done.

Keep each rule specific enough to verify from the repo. Remove generic, aspirational, stale, or duplicated guidance.

## Output

For architecture review, return:

1. `Question`
2. `Evidence`
3. `Recommendation`
4. `Tradeoffs`
5. `Staged next steps`
6. `AGENTS.md updates`, if relevant
7. `What not to change yet`

For direct AGENTS.md edits, summarize:

- files changed,
- why the guidance is repo-specific,
- validation performed,
- anything intentionally not documented.
