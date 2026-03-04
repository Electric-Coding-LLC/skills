# Prompt Quality Rubric

Use this rubric to diagnose prompt quality before execution.

## Scoring

Score each dimension from `0` to `2`.

- `0`: missing or unusable
- `1`: partially specified
- `2`: clear and actionable

Total score guidance:

- `0-4`: `blocked`
- `5-8`: `repairable`
- `9-12`: `ready`

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

## High-Leverage Clarifying Questions

Ask only questions that materially change the result.

1. "What exact outcome should the model produce?"
2. "Who is the audience for the output?"
3. "What constraints are mandatory (scope, tone, length, policies)?"
4. "What format should the output follow?"
5. "How will you judge whether the output is good enough?"

## Recheck Checklist

Before marking a prompt `ready`, ensure:

- Objective is explicit
- Inputs/context are named
- Constraints are listed
- Output format is concrete
- Quality bar is stated
