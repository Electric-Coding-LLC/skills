---
name: chunk
description: Identify the next logic chunk of implementation work that can be completed end-to-end without exceeding context limits. Use when a user asks what to build next, wants to break a large task into context-safe slices, or needs an execution order that preserves momentum without losing quality.
---

# Chunk

## Goal

Pick the highest-value next implementation slice that is small enough for one working session and complete enough to ship, test, and review without context overflow.
Prefer moderate slices: meaningful vertical progress that supports sustained coding, but remains bounded enough to fit in-context reasoning and verification.
If no justified implementation slice remains, say so explicitly instead of inventing more work.

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
- `verification surface`: can correctness be proven with targeted checks rather than broad manual QA.

4. Select the next chunk.
- Choose the highest-value chunk that still has high closure and low context risk.
- If none fit, split the top candidate again until it does.
- If the remaining work is speculative, low-value, or blocked on measurement, return `No justified next chunk` instead of forcing a continuation.

5. Define a completion contract before implementation.
- Include: files likely touched, acceptance criteria, tests, and explicit out-of-scope boundaries.
- Make the contract strict enough to avoid drift.

## Context-Fit Heuristics

Treat a chunk as context-safe only when most checks pass:

- Requires understanding 1 subsystem or one tightly-coupled subsystem pair, not the full architecture.
- Touches a bounded set of codepaths with one coherent purpose.
- Can be implemented and validated with targeted tests, not full-suite archaeology.
- Avoids simultaneous schema + API + UI rewrites in the same chunk.
- Does not depend on unresolved product or architecture decisions.

Moderate slice target:
- Usually one user-visible behavior or one meaningful backend capability.
- Can span more files when the dependency chain is already understood and verification remains narrow.
- Expected to fit in one focused implementation-and-verification pass.

If a candidate fails these checks, split by boundary:
- By layer: parser first, API later, UI last.
- By operation: read path first, then write path.
- By lifecycle: scaffolding first, behavior second, cleanup third.

## Output Contract

Return chunk recommendations in this order:

1. `Next chunk`
- One-sentence statement of the selected implementation slice.
- If appropriate, return `No justified next chunk` and make that the decision.

2. `Why this chunk now`
- Explain value, closure, and context-fit in 2 to 4 bullets.

3. `Definition of done`
- Acceptance criteria plus exact tests/checks to run.

4. `Out of scope for this chunk`
- List what must be deferred to avoid context blow-up.

5. `Follow-up chunks`
- List the next 1 to 3 chunks in execution order.
- If there is no justified continuation, say `None until profiling, user input, or a new requirement changes the value calculation.`

6. `Stop condition`
- State whether this track is `continue`, `measure first`, `blocked`, or `done for now`.
- If not continuing, say exactly what would justify reopening the track.

## Guardrails

- Avoid proposing chunks that end in "more investigation needed" without deliverable code or tests.
- Do not hide uncertainty. If dependencies are unknown, make that discovery task the chunk.
- Prefer thin vertical slices over horizontal "refactor everything first" plans.
- Prefer moderate vertical slices over tiny micro-tasks unless risk forces further split.
- Prefer bounded verification surface over arbitrary file-count limits.
- Optimize for completed increments, not local perfection.
- Treat stopping as a valid outcome when remaining work is speculative or not clearly worth the complexity.
- Do not propose more optimization work without profiling or another concrete signal when the obvious high-value cases are already done.
