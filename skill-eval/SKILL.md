---
name: skill-eval
description: Evaluate Codex skills for usefulness, overlap, trigger quality, installed-copy drift, and over-constraining language. Use when a user asks to review, audit, improve, simplify, merge, remove, or sync skills.
---

# Skill Eval

## Goal

Keep a skill set useful without letting it become process debt.

`$skill-eval` is for maintaining skills:
- trigger descriptions
- body instructions
- metadata alignment
- overlap between skills
- installed-copy sync
- keep, revise, merge, remove, or defer decisions

## Workflow

1. Define the evaluation scope.
- Identify the skill, skill group, repo, installed copy, or user concern.
- Preserve the user's exact concern: usefulness, over-triggering, missing coverage, overlap, sync, or deletion.

2. Inspect current artifacts.
- Read each relevant `SKILL.md`.
- Read `agents/openai.yaml` when present.
- Compare repo copies with installed copies when sync matters.
- Check line counts and obvious clutter.

3. Evaluate trigger quality.
- The frontmatter description should be specific enough to trigger when useful and stay quiet otherwise.
- Flag descriptions that are too broad, too narrow, stale, or inconsistent with the body.
- Check whether metadata default prompts add constraints not present in the skill.

4. Evaluate body quality.
- A useful skill should prevent a recurring failure mode.
- It should add the minimum process needed for that failure mode.
- It should preserve engineering judgment and repo truth.
- It should name boundaries with adjacent skills.

5. Classify the skill.
- `keep`: useful and not too constraining.
- `revise`: useful but wording, trigger, or boundaries need adjustment.
- `merge`: overlaps enough that one skill should absorb the other.
- `remove`: no distinct failure mode or not worth the overhead.
- `defer`: promising but needs real usage before deciding.

6. Apply changes only when requested.
- If the user asks for an audit, report findings without editing.
- If the user asks to update skills, patch repo sources first.
- Sync installed copies after repo changes when the user wants the local Codex install updated.

## Useful Checks

- `find . -maxdepth 2 -name SKILL.md -print | sort`
- `wc -l */SKILL.md */agents/openai.yaml`
- `rg -n "\\b(always|never|must|only|do not|explicitly|stop|required)\\b" */SKILL.md`
- `diff -rq <skill> "$HOME/.codex/skills/<skill>"`

Use search results as leads, not proof.

## Guardrails

- Do not optimize for having more skills.
- Do not add a global bootstrap skill unless the user explicitly asks for one.
- Do not turn a skill review into a philosophy document.
- Do not remove or rewrite skills just because they are blunt or opinionated.
- Do not sync installed copies until the repo source is the intended version.
- Do not treat installed-only skills as repo drift unless they are supposed to be managed by this repo.

## Output Contract

Return results in this order:

1. `Scope`
- Skills or installed copies reviewed.

2. `Findings`
- Concrete issues with file paths and why they matter.

3. `Disposition`
- `keep`, `revise`, `merge`, `remove`, or `defer` for each reviewed skill.

4. `Recommended changes`
- Smallest useful repo updates, or `None`.

5. `Sync state`
- Whether repo and installed copies match, were synced, or were intentionally left alone.
