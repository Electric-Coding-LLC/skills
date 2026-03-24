---
name: p1
description: Identify the single most important issue to work on in the current project from the repo and available context. First look for the most critical gap, failure, or risk that materially undermines the product. If nothing is seriously broken, choose the one issue whose fix would create the largest practical product improvement. Use when the user wants project-level prioritization, not artifact critique, implementation chunking, or a general review.
---

# P1

## Goal

Choose one repo-level issue to work on next.

Default decision rule:
- First, look for the most serious product gap, broken path, or project risk.
- If nothing is seriously wrong, pick the one issue whose fix would most improve the product for real users.

`$p1` is for prioritization, not broad critique.

## Workflow

1. Build a project-level view from available context.
- Use the repo, current workspace, docs, tests, configuration, issue clues, and recent changes that are already available.
- Prefer signals that reveal product reality: broken setup, failing checks, missing core flows, contradictory docs, obvious dead ends, severe usability gaps, or repeated workaround patterns.
- Do not require the user to provide a PRD, screenshot, diff, or bounded artifact first.

2. Check for serious problems first.
- Look for failures or gaps that make the product unreliable, misleading, unsafe, unshippable, or materially incomplete.
- Prefer issues that affect the main path, not niche polish.
- If a serious problem exists, stop ranking growth ideas and choose the strongest one.

3. If nothing is seriously broken, look for the highest-leverage improvement.
- Focus on the issue whose fix would most improve product usefulness, adoption, trust, or day-to-day user experience.
- Favor practical leverage over elegance, cleanup, or speculative architecture.
- Choose only one issue, even if several are defensible.

4. Sanity-check the pick.
- Make sure the issue is project-level, not just a comment on one file or artifact.
- Make sure it is a problem statement, not an implementation plan.
- Make sure it matters now, given the current repo context.

## What P1 Is Not

- Not a general review skill.
- Not a laundry-list generator.
- Not a diff critique or PR gate.
- Not a cleanup sweep for dead code or stale files.
- Not an implementation slicing or sequencing skill.

If another skill is a better fit, say so briefly:
- Use `$review` for code/diff readiness and verification risk.
- Use `$slop` for fallback hacks, stale guards, and workaround logic.
- Use `$clutter` for dead weight, unused files, and stale residue.
- Use `$chunk` after the priority is chosen and the user wants the next bounded implementation slice.

## Guardrails

- Return exactly one primary issue.
- Keep the tone practical and low-drama.
- Do not inflate minor imperfections into urgent problems.
- Do not hide uncertainty; if the evidence is thin, say so.
- Do not pad the answer with secondary opportunities unless they are necessary to explain why they lost.
- Prefer product impact over local code taste.

## Output Contract

Return results in this order:

1. `P1`
- One sentence naming the single issue to work on next.

2. `Why this is P1`
- 2 to 4 bullets covering the evidence, impact, and why it beats nearby alternatives.

3. `Why it matters now`
- One short paragraph grounded in current project context.

4. `Next move`
- One short paragraph stating the immediate action to take next without turning it into a full implementation plan.
