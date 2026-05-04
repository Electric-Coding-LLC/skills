---
name: truth
description: Determine the real current state of a repository by inspecting source-of-truth artifacts before answering status, release, completion, roadmap, or progress questions. Use when a user asks what is done, shipped, released, active, next, stale, or why repo status looks inconsistent.
---

# Truth

## Goal

Answer status questions from repo evidence instead of memory, assumptions, or stale plan text.

`$truth` is for repo-state reality checks:
- what is actually done
- what is active
- what shipped or released
- what is only planned
- why a progress artifact looks wrong
- what the next status-grounded move should be

It is not project prioritization or implementation slicing.
- Use `$p1` when the user wants the single most important project issue.
- Use `$chunk` when the user wants the next bounded implementation slice.

## Workflow

1. Identify the status question.
- Restate the exact claim or uncertainty being checked.
- Separate implementation state, merge state, release state, deploy state, and planning state.

2. Find the repo root and current git state.
- Run `git rev-parse --show-toplevel` when needed.
- Inspect branch, dirty state, ahead/behind state, and relevant local commits.
- Do not assume the current working directory is the repo root.

3. Read the source-of-truth artifacts.
- Prefer repo-native trackers such as `PLAN.md`, `EXECMAP.md`, `PROGRESS.md`, roadmap docs, changelogs, release docs, tags, package/app versions, CI config, and PR state.
- If the repo has a named active plan, read it before answering.
- If multiple artifacts disagree, treat the disagreement as the finding.

4. Classify the real state.
- Use precise labels:
  - `planned`
  - `active`
  - `implemented locally`
  - `committed`
  - `PR open`
  - `merged`
  - `released`
  - `deployed`
  - `stale or contradictory`
- Do not collapse these states into "done."

5. Answer with evidence and the next concrete move.
- Cite the files, commands, commits, or PRs used.
- If the user asks "what next?", name one grounded next action, not a backlog.
- If evidence is incomplete, say exactly what remains unknown.

## Guardrails

- Do not answer release, completion, or roadmap questions from memory alone when the repo is available.
- Do not mark planned work complete because a doc exists.
- Do not treat merged code as released unless release evidence exists.
- Do not create a new status artifact to explain old status unless the user asks.
- Do not fix tracker drift unless the user asks for updates; report the drift and the minimal correction.

## Output Contract

Return results in this order:

1. `Question checked`
- The status claim or uncertainty.

2. `Repo evidence`
- Files, git state, versions, tags, PRs, or commands inspected.

3. `Actual state`
- One precise status label with a short explanation.

4. `Mismatch`
- Any stale, contradictory, or missing tracker evidence. Say `None found` if none.

5. `Next move`
- One concrete action, or `No action needed` if the repo state is already coherent.
