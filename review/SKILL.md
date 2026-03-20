---
name: review
description: "Run a pre-Yeet quality gate on local code changes. Use when a user asks for a review, readiness check, or pre-PR check after making changes and before running Yeet. Perform three passes: (1) code review for bugs/regressions/tests, (2) security review for common vulnerabilities and secret exposure, and (3) style review for lint/format/convention drift. Return blocking issues, non-blocking suggestions, and an explicit ready-for-Yeet verdict."
---

# Review

## Goal

Assess working-tree changes before push/PR and decide whether it is safe to proceed to `$yeet`.
Focus on signal, not volume: identify concrete risks, explain impact, and avoid speculative noise.

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

3. Run a code review pass.
- Look for behavior changes, correctness bugs, regression risk, missing edge-case handling, and test gaps.
- Prioritize findings that can cause broken functionality, data loss, or operational incidents.
- Run the discovered repo-native checks that are the local equivalent of required PR gates for the changed scope.
- Prefer the narrowest command that still matches the repo's real gate; do not substitute lighter ad hoc checks when an exact local command exists.
- If a required check is not run, fails, or cannot run locally, treat that as blocking unless the repo genuinely has no local equivalent.

4. Run a security review pass.
- Check diffs for exposed secrets, insecure defaults, missing auth/authz controls, weak input validation, injection risks, unsafe deserialization, path traversal, and SSRF patterns where relevant.
- Flag risky dependency or configuration changes that lower security posture.
- For Python, JavaScript/TypeScript, or Go changes, apply language-appropriate secure coding checks.
- Keep findings actionable and mapped to exact files/lines.

5. Run a style review pass.
- Validate formatting, lint compliance, naming clarity, and consistency with repository conventions.
- Prefer check-only style commands where available (for example lint/format check modes).
- Do not auto-rewrite files unless the user asks for fixes.

6. Produce a decision and handoff.
- Classify each issue as:
  - `blocking`: must be fixed before `$yeet`
  - `non-blocking`: improvement suggestion
- Any failed, skipped, unavailable, or weaker-than-required verification command keeps the review in `blocking`.
- End with explicit verdict:
  - `Ready for Yeet: yes`
  - `Ready for Yeet: no`
- If ready, include the handoff line: `Proceed with $yeet.`

## Guardrails

- Do not commit, push, or run `$yeet` as part of this skill unless the user explicitly asks.
- Do not hide uncertainty. If the check could not be run, say exactly what was skipped.
- Distinguish `unsupported in this repo`, `not available in environment`, and `skipped` because those imply different residual risks.
- Do not mark the diff ready when required repo-native checks were skipped, replaced with lighter commands, or left unavailable without a documented no-local-equivalent reason.
- Do not flood the user with low-value nits when blocking issues exist.
- Keep findings concrete: include file path and line references where possible.

## Output Contract

Return results in this order:

1. `Blocking issues`
2. `Non-blocking suggestions`
3. `Checks run` (exact commands and outcomes for tests/lint/typecheck/build/security checks, plus anything unsupported, unavailable, or skipped with reason)
4. `Ready for Yeet: yes|no`

When no blocking issues remain, include: `Proceed with $yeet.`
