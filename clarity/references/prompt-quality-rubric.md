# Prompt Quality Rubric

Use this rubric to diagnose prompt quality before execution. Evaluate the prompt together with available conversation, instructions, and project evidence; do not require the user to repeat discoverable facts.

## Scoring

Use the dimensions as diagnostic prompts. An optional `0` to `2` score can help with a detailed audit, but a numeric total must not override whether missing information actually prevents useful execution.

- `0`: missing or unusable
- `1`: partially specified
- `2`: clear and actionable

Classify as `ready` when the available context is sufficient, `repairable` when a material clarification would improve the result, and `blocked` only when unresolved intent or contradictions prevent safe execution.

## Dimensions

1. Objective clarity
- Check whether the prompt states a concrete outcome.
- Reject goals like "make it better" without defining "better."

2. Context sufficiency
- Check whether required background, source data, or constraints are present.
- Flag hidden assumptions likely to reduce output quality.

3. Constraints and boundaries
- Check for scope limits, tone requirements, and non-goals.
- Require explicit constraints when precision matters.

4. Output contract
- Check for desired format, length, structure, and audience.
- Require a clear deliverable shape.

5. Quality bar
- Check for acceptance criteria or quality checks.
- If missing, flag that quality will be inconsistent.

6. Failure-mode controls
- Check for instructions that reduce hallucination or drift when relevant.
- Add a risk note if factual reliability requirements are absent.

## Coding Prompt Checks

When the prompt is for implementation work, also verify:

1. Repo context
- Names the repo area, files, modules, or entry points to inspect.

2. Execution environment
- States relevant tool, permission, runtime, or sandbox constraints.

3. Verification
- Specifies exact tests, linters, or checks to run.

4. Reporting contract
- States what the agent should return: changed files, checks run, blockers, risks, or follow-ups.

These details may come from the prompt, session, repo instructions, or project configuration. Their omission from the prompt alone does not prevent a `ready` verdict. Recommend discovery for routine technical details; ask a question only when a missing answer materially affects scope, correctness, or the user's intended result.

## High-Leverage Clarifying Questions

Ask only questions that materially change the result.

1. "What exact outcome should the model produce?"
2. "Who is the audience for the output?"
3. "What constraints are mandatory (scope, tone, length, policies)?"
4. "What format should the output follow?"
5. "How will you judge whether the output is good enough?"

## Recheck Checklist

Before marking a prompt `ready`, ensure:

- The intended outcome is clear enough to act on.
- Necessary inputs are available or have a practical discovery path.
- Material constraints and the quality bar are known or safely inferable.
- No unresolved choice would materially change the requested result.
