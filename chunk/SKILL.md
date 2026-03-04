---
name: chunk
description: Identify the next logic chunk of implementation work that can be completed end-to-end without exceeding context limits. Use when a user asks what to build next, wants to break a large task into context-safe slices, or needs an execution order that preserves momentum without losing quality.
---

# Chunk

## Goal

Pick the highest-value next implementation slice that is small enough for one working session and complete enough to ship, test, and review without context overflow.
Prefer moderate slices: meaningful vertical progress that supports sustained coding, but remains bounded enough to fit in-context reasoning and verification.

## Workflow

1. Define the target outcome.
- Restate the user-visible outcome in one sentence.
- Identify hard constraints: deadlines, quality bar, required tests, and no-go areas.

2. Build a candidate chunk list.
- Scan the current code/task and enumerate 3 to 7 candidate chunks.
- Prefer chunks that map to one clear behavior change.
- Target moderate slices first; split only when a candidate risks context overload.

3. Score each chunk for context fit and completion likelihood.
- `scope`: can it be understood from a small local surface area.
- `closure`: can it include code, tests, and verification in one pass.
- `risk`: dependency unknowns, cross-module blast radius, migration coupling.
- `value`: user-visible impact or unblock potential.

4. Select the next chunk.
- Choose the highest-value chunk that still has high closure and low context risk.
- If none fit, split the top candidate again until it does.

5. Define a completion contract before implementation.
- Include: files likely touched, acceptance criteria, tests, and explicit out-of-scope boundaries.
- Make the contract strict enough to avoid drift.

## Context-Fit Heuristics

Treat a chunk as context-safe only when most checks pass:

- Requires understanding 1 subsystem or one tightly-coupled subsystem pair, not the full architecture.
- Touches a moderate number of files with one coherent purpose.
- Can be implemented and validated with targeted tests, not full-suite archaeology.
- Avoids simultaneous schema + API + UI rewrites in the same chunk.
- Does not depend on unresolved product or architecture decisions.

Moderate slice target:
- Usually one user-visible behavior or one meaningful backend capability.
- Often 4 to 10 files or one concentrated module area.
- Expected to take a focused work block, not a quick patch.

If a candidate fails these checks, split by boundary:
- By layer: parser first, API later, UI last.
- By operation: read path first, then write path.
- By lifecycle: scaffolding first, behavior second, cleanup third.

## Output Contract

Return chunk recommendations in this order:

1. `Next chunk`
- One-sentence statement of the selected implementation slice.

2. `Why this chunk now`
- Explain value, closure, and context-fit in 2 to 4 bullets.

3. `Definition of done`
- Acceptance criteria plus exact tests/checks to run.

4. `Out of scope for this chunk`
- List what must be deferred to avoid context blow-up.

5. `Follow-up chunks`
- List the next 1 to 3 chunks in execution order.

## Guardrails

- Avoid proposing chunks that end in "more investigation needed" without deliverable code or tests.
- Do not hide uncertainty. If dependencies are unknown, make that discovery task the chunk.
- Prefer thin vertical slices over horizontal "refactor everything first" plans.
- Prefer moderate vertical slices over tiny micro-tasks unless risk forces further split.
- Optimize for completed increments, not local perfection.
