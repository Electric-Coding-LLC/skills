---
name: review
description: "Review an intended working-tree change, commit, branch, or pull request for slop, correctness, security, style, and delivery readiness. Use for code review or a pre-PR quality gate; resolve the requested scope before choosing the diff."
---

# Review

## Goal

Assess the complete intended change and decide whether it is safe to proceed to `$sendit`.
Focus on signal, not volume: identify concrete risks, cleanup debt that would make the change harder to own, explain impact, and avoid speculative noise.

## Workflow

1. Resolve and capture review scope.
- Honor an explicit file, working-tree, commit, branch, or PR scope. Otherwise inspect repository state and infer the intended change from the task and current branch; state the selected scope and base.
- Use `git status --short`, `git diff`, and `git diff --cached` to identify local changes. Inspect relevant untracked files from `git ls-files --others --exclude-standard`; ordinary diffs omit them. Include deletions and renames.
- For a branch or PR, resolve its actual target branch, then inspect the committed change with `git diff <base>...HEAD` (or the named head). Do not assume an empty working tree means an empty change. For a single commit, inspect that commit's patch; resolve the intended parent for a merge commit.
- When reviewing a pending local delivery, include its intended committed, staged, unstaged, and untracked changes. Keep unrelated local work outside the review. For a named remote PR, inspect its actual revision and distinguish it from a potentially different local checkout.
- If the base or intended changes cannot be determined safely, ask only for that missing scope. Report "nothing to review" only after checking the entire selected scope.

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
- Reuse passing results only when they cover the reviewed content and relevant environment; rerun affected checks after changes. Do not claim local checks prove a different remote revision.
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
