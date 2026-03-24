---
name: clutter
description: Detect low-value residue in a codebase by finding things that are no longer needed, not referenced, duplicated, stale, or just noise. Use when a user asks for cleanup, deletion candidates, dead-code review, stale asset/doc/script detection, or help separating useful structure from junk. Prefer evidence-backed findings and classify them as safe to remove, likely stale, or needs confirmation.
---

# Clutter

## Goal

Find things that add little or no present value: dead files, unused code, stale docs, duplicate helpers, abandoned fixtures, noisy comments, and other residue that makes the repo harder to read or trust.

`$clutter` is not `$slop`.
- Use `$clutter` for dead weight and noise.
- Use `$slop` for fallback hacks, stale guards, compatibility branches, and unblocker logic that changes behavior or hides defects.

## Workflow

1. Define scope.
- Start with the files, package, feature area, or diff the user cares about.
- Prefer local evidence over full-repo wandering.
- If the requested scope is broad, identify the highest-density clutter area first instead of scanning everything shallowly.

2. Scan for clutter candidates.
- `dead code`: unused exports, helpers, components, styles, tests, examples, or fixtures.
- `dead files`: files with no references, obsolete scripts, archived migrations, abandoned experiments.
- `duplicate residue`: near-duplicate utilities, copied constants, repeated helpers, overlapping docs.
- `stale docs/config`: instructions, env vars, commands, flags, or config blocks that no longer match the repo.
- `noise`: comments, TODOs, placeholders, sample data, logs, or wrappers that do not improve comprehension or behavior.

3. Collect proof before recommending deletion.
- Check references with repo-native search.
- Inspect nearby imports, call sites, package scripts, tests, docs, and CI config.
- Distinguish "not referenced here" from "safe to remove."
- When uncertainty remains, downgrade the finding instead of forcing a deletion recommendation.

4. Assign disposition.
- `safe to remove`: strong evidence it is unused or redundant.
- `likely stale`: evidence suggests low value, but one assumption still needs confirmation.
- `needs confirmation`: suspicious clutter, but ownership/runtime usage is unclear.
- `keep`: reviewed and justified; explain why it still earns its space.

5. Validate safety.
- For code deletions, run the narrowest relevant checks available.
- For docs/config cleanup, verify there is no live reference path in scripts, CI, or deployment metadata.
- State residual risk when validation is partial.

## Detection Heuristics

Use search as lead generation, not proof:
- `rg -n "(TODO|FIXME|XXX|deprecated|obsolete|unused|remove me|remove this|cleanup)"`
- `rg -n "(console\\.log|print\\(|dbg!|fmt\\.Println)"` in non-test code
- `rg -n "(example|sample|tmp|temp|backup|old|copy|draft)"` on filenames and comments
- `rg --files | rg "(spec|fixture|mock|example|sample|story|stories|snapshot)"`
- `rg -n "export "` or language-specific symbol definitions, then verify actual references

Useful clues:
- files with no imports or call sites
- docs that mention commands or paths that no longer exist
- duplicate helpers with one dominant call path
- compatibility notes for systems no longer supported
- tests or fixtures for deleted features

## Guardrails

- Do not recommend deleting anything solely because it looks ugly, verbose, or unfamiliar.
- Do not remove assets, configs, or scripts with unclear deployment/runtime ownership without saying what remains unknown.
- Do not treat low-frequency usage as no usage; confirm whether the item is intentionally dormant.
- Prefer consolidation over deletion when two artifacts both still serve active callers.
- When the real issue is behavioral complexity rather than dead weight, hand the finding to `$slop`.

## Output Contract

Return results in this order:

1. `Safe to remove`
- Include file path or symbol, evidence, expected impact, and any validation run.

2. `Likely stale`
- Include what points to staleness and what still needs confirmation.

3. `Needs confirmation`
- Include the missing ownership or runtime evidence that blocks a stronger call.

4. `Reviewed and kept`
- List suspicious items that were checked and kept, with rationale.

5. `Checks run`
- Tests/checks/searches executed, plus anything skipped.

6. `Clutter verdict`
- `high`, `medium`, or `low` clutter in the reviewed scope, with one-sentence justification.
