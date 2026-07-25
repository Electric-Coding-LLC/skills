---
name: review
description: "Run a pre-Sendit quality gate on local code changes. Use when a user asks for a review, readiness check, or pre-PR check after making changes and before running Sendit. Perform four passes: (1) slop review for unnecessary fallbacks, stale guards, compatibility leftovers, and unblocker hacks, (2) code review for bugs/regressions/tests, (3) security review for common vulnerabilities and secret exposure, and (4) style review for lint/format/convention drift. Return blocking issues, non-blocking suggestions, and an explicit ready-for-Sendit verdict."
---

# Review

## Goal

Assess working-tree changes before push/PR and decide whether it is safe to proceed to `$sendit`.
Focus on signal, not volume: identify concrete risks, cleanup debt that would make the change harder to own, explain impact, and avoid speculative noise.

## Workflow

1. Capture review scope.
- Inspect repository state:
  - `git status --short`
  - `git diff --stat`
  - `git diff --name-only --diff-filter=ACMR`
- Include both unstaged and staged changes:
  - `git diff`
  - `git diff --cached`
- If there are no changes, report "nothing to review" and stop.

2. Discover repo-native verification commands before running checks.
- Inspect local project files such as `package.json`, `Makefile`, `justfile`, `pyproject.toml`, `go.mod`, and CI config.
- Prefer commands the repo already defines over generic guesses.
- Map local commands to the checks that gate pull requests when that is discoverable from CI config.
- If no project-specific commands are discoverable, say that explicitly and fall back cautiously.

3. Run a slop review pass.
- Look for unnecessary fallbacks, stale guards, compatibility leftovers, and unblocker hacks in the changed code plus immediate call sites.
- Treat grep hits such as `TODO`, `FIXME`, `HACK`, `workaround`, `temporary`, `legacy`, `compat`, broad `catch` blocks, and silent empty defaults as lead generation, not proof.
- Classify slop as `blocking` when it can hide defects, weaken invariants, preserve invalid legacy behavior, or make the changed path harder to reason about before shipping.
- Classify slop as `non-blocking` when it is cleanup-only and does not materially affect the delivery decision.
- Keep necessary protections at untrusted boundaries, auth/authz checks, filesystem access, serialization, and external integration boundaries unless there is proof they are redundant.

4. Run a code review pass.
- Look for behavior changes, correctness bugs, regression risk, missing edge-case handling, and test gaps.
- Prioritize findings that can cause broken functionality, data loss, or operational incidents.
- Run the discovered repo-native checks that are the local equivalent of required PR gates for the changed scope.
- Prefer the narrowest command that still matches the repo's real gate; do not substitute lighter ad hoc checks when an exact local command exists.
- If a required check is not run, fails, or cannot run locally, treat that as blocking unless the repo genuinely has no local equivalent.

5. Run a security review pass.
- Check diffs for exposed secrets, insecure defaults, missing auth/authz controls, weak input validation, injection risks, unsafe deserialization, path traversal, and SSRF patterns where relevant.
- Flag risky dependency or configuration changes that lower security posture.
- For Python, JavaScript/TypeScript, or Go changes, apply language-appropriate secure coding checks.
- Keep findings actionable and mapped to exact files/lines.

6. Run a style review pass.
- Validate formatting, lint compliance, naming clarity, and consistency with repository conventions.
- Prefer check-only style commands where available (for example lint/format check modes).
- Do not auto-rewrite files unless the user asks for fixes.

7. Produce a decision and handoff.
- Classify each issue as:
  - `blocking`: must be fixed before `$sendit`
  - `non-blocking`: improvement suggestion
- Any failed, skipped, unavailable, or weaker-than-required verification command keeps the review in `blocking`.
- Any blocking slop finding keeps the review in `blocking`.
- End with explicit verdict:
  - `Ready for Sendit: yes`
  - `Ready for Sendit: no`
- If ready, include the handoff line: `Proceed with $sendit.`

## Guardrails

- Do not commit, push, or run `$sendit` as part of this skill unless the user explicitly asks.
- Do not hide uncertainty. If the check could not be run, say exactly what was skipped.
- Distinguish `unsupported in this repo`, `not available in environment`, and `skipped` because those imply different residual risks.
- Do not mark the diff ready when required repo-native checks were skipped, replaced with lighter commands, or left unavailable without a documented no-local-equivalent reason.
- Do not mark the diff ready while blocking slop findings remain.
- Do not flood the user with low-value nits when blocking issues exist.
- Keep findings concrete: include file path and line references where possible.
- Use standalone `$slop` only when the user wants a focused cleanup-only audit or a deeper slop scan outside the normal review gate.

## Output Contract

Return results in this order:

1. `Blocking issues`
- Include blocking slop findings with file path, impact, and recommended fix.

2. `Non-blocking suggestions`
- Include non-blocking cleanup or slop findings only when they are worth addressing.

3. `Confirmed necessary protections`
- Include suspicious fallback/guard/compat code that was reviewed and intentionally kept, only when relevant.

4. `Checks run`
- Exact commands and outcomes for tests/lint/typecheck/build/security checks, plus anything unsupported, unavailable, or skipped with reason.

5. `Ready for Sendit: yes|no`

When no blocking issues remain, include: `Proceed with $sendit.`
