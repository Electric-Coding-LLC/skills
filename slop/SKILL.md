---
name: slop
description: Detect and remove unblocker-style code debt by auditing unnecessary fallbacks, redundant guards, stale backward-compatibility branches, and temporary workaround hacks that mask root causes. Use when a user asks for cleanup, simplification, post-hotfix hardening, or verification that defensive code is still justified before merge.
---

# Slop

## Goal

Find and remove code that was added to unblock delivery but now adds complexity, hides defects, or preserves legacy behavior that no longer matters.

## Workflow

1. Define scope.
- Review the exact files, diff, or module boundaries first.
- Prefer changed code plus immediate call sites over full-repo scanning.
- If the diff-local scan shows no meaningful slop candidates, say so and stop instead of broadening scope automatically.

2. Run a slop scan across four categories.
- `fallback slop`: silent defaults, catch-all return values, error swallowing, or secondary code paths with no current business need.
- `guard slop`: null/state checks that are impossible given current types, contracts, or validated boundaries.
- `compat slop`: legacy API shims, old-version branches, migration bridges, and feature flags that should have been retired.
- `unblocker hacks`: temporary workarounds, deadline patches, "TODO remove" logic, or bypasses that avoid root-cause fixes.

3. Collect proof before recommending removal.
- Explain why the code likely exists.
- Verify if the original constraint still applies (runtime contract, rollout phase, supported version matrix, external dependency behavior).
- Check current call sites and tests for evidence of real usage.

4. Decide disposition per finding.
- `remove`: unnecessary branch or fallback with no valid current use.
- `replace`: swap workaround for explicit invariant checks, stricter typing, or a real root-cause fix.
- `keep`: still required; add rationale and expiry conditions if temporary.

5. Validate safety.
- Run targeted tests or checks for touched areas.
- If no tests are available, state residual risk explicitly.

## Detection Heuristics

Use grep as lead generation, not proof:
- `rg -n "(TODO|FIXME|HACK|workaround|temporary|temp|unblock|hotfix|legacy|compat|fallback|remove after)"`
- `rg -n "(try\\s*\\{)|(catch\\s*\\()"` and inspect for swallowed or diluted errors
- `rg -n "(if\\s*\\(.*\\)\\s*return\\s*(null|undefined|\\{\\}|\\[\\]|false|0|\"\"))"`
- `rg -n "(v1|v2|legacy|deprecated|backward|backwards|compat)"` on critical paths

Treat every match as a hypothesis that needs context.

## Guardrails

- Do not remove protections at untrusted boundaries (network input, auth/authz, filesystem, serialization) without replacement.
- Do not remove compatibility paths unless support policy and rollout state confirm it is safe.
- Prefer explicit failure for impossible states over silent fallback behavior.
- Avoid cosmetic cleanup recommendations that do not reduce real risk or complexity.

## Output Contract

Return results in this order:

1. `Blocking slop`
- Include file path and line reference, why it is slop, impact, and recommended fix.

2. `Non-blocking cleanup`
- Include simplifications that improve maintainability but are not immediate risk.

3. `Confirmed necessary protections`
- List suspicious items that were reviewed and intentionally kept, with rationale.

4. `Checks run`
- Tests/checks executed, plus skipped checks with reason.

5. `Slop verdict`
- `high`, `medium`, or `low` slop risk for the reviewed scope, with one-sentence justification.
