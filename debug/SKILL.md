---
name: debug
description: Investigate and fix bugs by reproducing the symptom, tracing the root cause, and applying the narrowest correct fix without adding fallback hacks or masking invalid state. Use when a user asks to debug, diagnose, root-cause, fix a failing behavior, or explain why something is broken.
---

# Debug

## Goal

Find the real cause of a failure and fix it without turning the symptom into technical debt.

`$debug` is not TDD and not cleanup.
- Use `$debug` for failures, regressions, broken flows, confusing runtime behavior, and failing checks.
- Use `$slop` when the main question is whether fallback hacks or guards should be removed.
- Use `$review` when the main question is whether an existing diff is ready to ship.

## Workflow

1. Define the failure.
- State the observed symptom in concrete terms.
- Identify the expected behavior.
- Capture the smallest useful reproduction path: command, test, UI action, log, input, or state.

2. Reproduce or constrain the symptom.
- Prefer a direct reproduction over code reading alone.
- If full reproduction is blocked, gather the nearest reliable evidence and state the gap.
- Do not patch before the failing boundary is understood.

3. Trace the root cause.
- Follow the real call path, data flow, lifecycle, or state transition.
- Identify the first boundary where expected state becomes wrong.
- Distinguish cause from consequence.
- Check recent changes, config, environment, fixtures, and external assumptions when relevant.

4. Choose the narrowest correct fix.
- Fix the cause, not the nearest visible symptom.
- Keep the blast radius limited, but change the right boundary even if it touches more files.
- Preserve or strengthen invariants.
- Prefer explicit failure for impossible states over silent recovery.
- Avoid fallbacks, catch-all guards, compatibility branches, or "just in case" logic unless they are the actual product requirement.

5. Verify the original symptom.
- Rerun the reproduction or the closest reliable proof.
- Add or update regression coverage when it would catch the bug again with reasonable cost.
- Run targeted checks for touched areas.
- State residual risk if reproduction or verification is partial.

## Guardrails

- Do not equate "smallest fix" with fewest changed lines.
- Do not add default values, broad null guards, retries, swallowed errors, or alternate paths to make the symptom disappear.
- Do not remove safety checks at untrusted boundaries without replacement.
- Do not broaden into unrelated cleanup unless the cleanup is needed for the root-cause fix.
- Do not claim the bug is fixed without rerunning the original symptom or explaining why that proof was unavailable.

## Output Contract

Return results in this order:

1. `Symptom`
- What failed and how it was reproduced or constrained.

2. `Root cause`
- The specific boundary or assumption that was wrong.

3. `Fix`
- What changed and why it fixes the cause without masking invalid state.

4. `Verification`
- Exact commands, tests, reproduction steps, or evidence used.

5. `Residual risk`
- Anything not proven, skipped, blocked, or worth watching.
