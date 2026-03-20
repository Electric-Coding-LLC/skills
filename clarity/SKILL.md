---
name: clarity
description: Check user prompts for vagueness, ambiguity, and logical gaps that can lead to poor AI results. Use when a user asks to evaluate, critique, or pressure-test a prompt before running it, especially when the request is broad, underspecified, or likely to produce weak output. Focus on diagnosis and clarification questions rather than rewriting the prompt end-to-end.
---

# Clarity

## Goal

Act as a language and logic quality gate for prompts.
Identify why a prompt may produce poor results and what must be clarified.

## Scope

- In scope:
  - Diagnose prompt quality and risk
  - Flag vagueness and logic gaps
  - Ask high-leverage clarifying questions
- Out of scope:
  - Rewrite the full prompt by default
  - Execute the user task itself
  - Invent missing constraints without labeling assumptions

## Workflow

1. Classify the prompt type first.
- Use one of:
  - `coding`
  - `writing`
  - `analysis`
  - `automation`

2. Classify prompt quality quickly.
- Use three states:
  - `ready`: specific enough to run with low risk
  - `repairable`: usable after clarifications
  - `blocked`: too vague or contradictory to run safely

3. Run an ambiguity scan.
- Use the rubric in `references/prompt-quality-rubric.md`.
- Check for weak or missing elements:
  - Objective
  - Audience
  - Context or inputs
  - Constraints and boundaries
  - Output contract
  - Quality bar
- For `coding` prompts, also check:
  - Repo or code context
  - Allowed tools or environment constraints
  - Verification commands
  - Reporting format

4. Explain likely failure modes.
- For each major gap, state the likely output problem.
- Keep findings concrete and tied to quality impact.

5. Ask targeted clarifying questions.
- Ask only questions that materially change output quality.
- Prefer 3 to 5 questions maximum in the first pass.
- Skip questions when state is `ready`.

6. Provide minimal fix hints.
- Provide concise guidance on what to add.
- Do not provide a full rewritten prompt unless the user explicitly asks.

## Pushback Rules

Enforce quality without being hostile.

- Do not accept requests like "make this better" without defining "better."
- Do not proceed silently when core requirements are missing.
- Do not ask broad, low-value questions.
- Do not over-interrogate prompts already marked `ready`.
- State uncertainty clearly when key details are absent.

## Response Format

Return results in this order:

1. `Prompt quality verdict`
- Classify as `ready`, `repairable`, or `blocked`.
- Include the prompt type and one-sentence rationale.

2. `Vagueness and risk findings`
- List the top gaps (max 5).
- For each: `gap -> likely poor-result outcome`.

3. `Clarifying questions`
- Ask targeted questions only if needed.
- Skip this section when state is `ready`.

4. `Minimal fix hints`
- List the specific fields the user should add.
- Keep this short and structural.

## Example Triggers

- "Check this prompt for ambiguity before I run it."
- "Tell me if this request is too vague."
- "Find what in this prompt could cause a poor result."
- "Pressure-test this prompt's logic and constraints."

## Resource Use

Read `references/prompt-quality-rubric.md` when scoring prompt quality and drafting clarifying questions.

## Guardrails

- Avoid shaming language.
- Keep pushback direct and factual.
- Optimize for actionable feedback, not verbosity.
