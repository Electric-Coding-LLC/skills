---
name: housekeeping
description: Perform low-risk repository housekeeping by syncing stale status docs, fixing obvious command/path drift, cleaning accidental generated or temporary residue, and tightening small metadata/config inconsistencies. Use when the user asks for repo tidying, maintenance, status-doc cleanup, README or command alignment, lightweight docs/config truth fixes, or pre-delivery housekeeping that should not change product behavior.
---

# Housekeeping

## Goal

Keep the repo easy to trust by fixing small maintenance drift without changing
product behavior.

`$housekeeping` is not `$clutter`, `$slop`, `$review`, or dependency work.
- Use `$housekeeping` for routine maintenance and repo-truth alignment.
- Use `$clutter` for dead files, unused code, stale residue, and deletion candidates.
- Use `$slop` for fallback hacks, stale guards, compatibility branches, and workaround logic.
- Use `$review` for pre-PR readiness, security, style, and verification.

## Workflow

1. Define scope.
- Identify the repo area, docs, config, plan, checklist, or diff being tidied.
- Prefer the smallest scope that removes current confusion.
- Do not turn housekeeping into broad refactoring or dependency updating.

2. Find housekeeping candidates.
- `status drift`: `PLAN.md`, `EXECMAP.md`, roadmap, checklist, changelog, or README status no longer matches repo truth.
- `command drift`: docs mention commands, scripts, env vars, or paths that no longer exist.
- `metadata drift`: package names, descriptions, scripts, labels, config comments, or ownership notes are stale.
- `generated residue`: temporary files, logs, local artifacts, copied outputs, cache files, or empty folders accidentally left in the repo.
- `small consistency fixes`: broken links, renamed paths, stale references, or obvious wording mismatches that reduce trust.

3. Prove the drift.
- Inspect the relevant source of truth before editing.
- Use repo search and project files to confirm paths, commands, scripts, status, or ownership.
- Distinguish obvious maintenance drift from uncertain product, release, or architecture decisions.

4. Apply narrow fixes.
- Update existing docs, config, checklists, and progress artifacts in place.
- Remove only clearly accidental generated or temporary residue.
- Do not delete source code, assets, scripts, or tests unless `$clutter` would classify them as safe to remove.
- Do not change runtime behavior unless it is necessary to correct a broken command/config reference and can be verified narrowly.
- Do not create new planning docs unless the user asks or the repo's existing process requires them.

5. Verify.
- For docs/status changes, check links, paths, commands, and tracker consistency.
- For config/script changes, run the cheapest relevant command.
- If verification is partial, state the residual risk.

## Guardrails

- Do not rewrite docs for style when the issue is factual drift.
- Do not make broad formatting changes across unrelated files.
- Do not perform package updates, lockfile refreshes, framework upgrades, runtime upgrades, or dependency migrations.
- Do not use housekeeping to sneak in refactors.
- Do not create a second source of truth when an existing plan, roadmap, or checklist should be updated.
- Hand deletion-heavy findings to `$clutter`.
- Hand behavioral workaround cleanup to `$slop`.

## Output Contract

Return results in this order:

1. `Housekeeping scope`
- What was tidied and what was intentionally out of scope.

2. `Fixed`
- Files or artifacts updated, with the drift corrected.

3. `Left alone`
- Suspicious items reviewed but not changed, with reason.

4. `Checks run`
- Exact searches, commands, or manual consistency checks used.

5. `Residual risk`
- Anything not proven, skipped, blocked, or worth watching.

6. `Next cleanup candidate`
- One follow-up only when it is concrete and still in housekeeping scope; otherwise say `None`.
