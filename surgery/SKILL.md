---
name: surgery
description: Use when the user explicitly asks for surgery, or for bug fixes, hot fixes, and repair work where the goal is to restore correctness with the smallest cohesive, design-consistent change.
---

# Surgery Skill

Use this workflow for repair-oriented coding tasks where the user wants a proper fix with minimal effect on unrelated areas.

This skill is not the default workflow for ordinary feature implementation. General implementation work may require moving code, reorganizing modules, refactoring, or improving the system shape when the task justifies it.

## Objective

Repair the real problem with the smallest cohesive, design-consistent change that preserves surrounding architecture and behavior.

## Process

1. Read the active `AGENTS.md` instructions.
2. Reproduce or identify the failure, regression, unsafe behavior, or broken invariant.
3. Inspect the nearest relevant implementation and nearby tests.
4. Identify the user's underlying goal, not just the proposed patch.
5. Determine the narrowest proper fix that addresses the root cause.
6. Reuse existing types, utilities, fixtures, boundaries, naming, and patterns.
7. Modify only files required for the repair.
8. Run targeted validation that proves the repaired behavior.
9. Review the diff for unnecessary structure before finishing.

## Authority

Follow the active `AGENTS.md` files and repo-specific instructions as the source of engineering policy. This skill provides the execution rhythm for repair work; it does not replace or restate the global rules.

When the user proposes a questionable implementation, preserve the user's goal but choose the design-consistent fix unless the user explicitly requires the original approach.

## Boundaries

- Do not use this skill for ordinary feature implementation just because code will be edited.
- Do not use this skill for broad refactors, architecture planning, system redesign, or exploratory restructuring unless the user explicitly asks for surgery.
- Use `$debug` when the primary task is to reproduce, root-cause, and fix a failure.
- Use `$review` when the primary task is to evaluate an existing diff before delivery.
- Use `$slop` when the primary task is to remove fallback hacks, workaround guards, or compatibility residue.
- Use `$flow` when the user wants chunking, implementation, cleanup, review, PR, merge, and post-merge cleanup as one delivery cycle.

## Diff Review Checklist

Before finishing, inspect the diff and remove:

- unrelated cleanup,
- unused helpers,
- single-use abstractions,
- unnecessary contracts or interfaces,
- unnecessary DTO/model/mapper layers,
- duplicate tests,
- implementation-detail tests,
- formatting-only churn,
- file moves unrelated to the task,
- dependencies not explicitly approved.
