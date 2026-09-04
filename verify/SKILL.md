---
name: verify
description: Choose and run the cheapest reliable proof for a change or claim before calling it done. Use when a user asks to verify, validate, smoke test, confirm completion, prove a fix, or check whether a change works without requiring test-first development.
---

# Verify

## Goal

Prove the relevant claim with the least ceremony that is still reliable.

`$verify` is evidence-driven, not TDD:
- tests are useful when they catch real regression risk
- build, lint, typecheck, reproduction scripts, logs, browser QA, screenshots, or manual checks may be better proof
- the proof should match the claim being made

It is not a full code review.
- Use `$review` when the user wants readiness, security, and style assessment before `$sendit`.

## Workflow

1. State the claim.
- Name the behavior, fix, release state, UI state, or invariant being verified.
- If the claim is vague, narrow it before running checks.

2. Choose the proof.
- Prefer repo-native commands and existing gates when they match the claim.
- Use the cheapest reliable proof, not the largest available suite by default.
- For UI work, prefer browser or screenshot evidence when rendering, layout, focus, or interaction matters.
- For bug fixes, verify the original symptom when possible.
- For docs or tracker changes, verify links, commands, paths, and status consistency.

3. Run the proof.
- Execute the selected checks.
- Capture exact commands, paths, inputs, or manual steps.
- If a proof is blocked by missing tools, services, credentials, or environment, say so directly.

4. Interpret the result.
- Distinguish `passed`, `failed`, `blocked`, `partial`, and `not applicable`.
- Do not treat unrelated passing checks as proof of the specific claim.
- If a check fails, identify whether it invalidates the claim or is unrelated.

5. Decide whether more proof is needed.
- Stop when the claim has reliable evidence.
- Add a targeted follow-up check only when it covers a real remaining risk.
- Avoid exhaustive verification when risk is already bounded.

## Guardrails

- Do not require test-first development.
- Do not claim done from inspection alone when executable proof is available and relevant.
- Do not replace a repo-native required gate with a weaker ad hoc command when readiness is the claim.
- Do not run broad expensive suites by reflex when a targeted proof is enough.
- Do not hide skipped or unavailable checks.

## Output Contract

Return results in this order:

1. `Claim`
- What was verified.

2. `Proof selected`
- Why this proof matches the claim.

3. `Checks run`
- Exact commands, steps, or evidence gathered.

4. `Result`
- `passed`, `failed`, `blocked`, `partial`, or `not applicable`, with concise interpretation.

5. `Remaining risk`
- Anything not covered and whether it matters.
