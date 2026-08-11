---
name: options
description: Generate a small set of useful, materially distinct, and feasible choices grounded in the user's goal and available context. Use when the user asks for options, alternatives, suggestions, directions, approaches, possibilities, examples to choose from, or what they could do next before committing to one path. Do not use for a request to execute an already chosen path, identify exactly one priority, or produce an exhaustive unranked brainstorm.
---

# Options

## Goal

Turn an open choice into a compact set of options the user can actually decide among.

## Workflow

1. Frame the choice.
- Identify the desired outcome, relevant constraints, existing preferences, and decision horizon.
- Inspect available artifacts or project context when they materially affect feasibility.
- Make a safe, reversible assumption when context is thin and label it briefly. Ask only when the missing answer would substantially change the option set.

2. Choose the option mode.
- `Alternatives`: competing approaches to one decision.
- `Candidates`: suggestions or examples that may be selected or combined.
- `Next moves`: bounded actions that create value or reduce uncertainty.

3. Build a meaningful choice space.
- Identify the few dimensions that change the outcome, such as scope, speed, cost, risk, reversibility, ambition, or user impact.
- Make each option differ on at least one important dimension.
- Screen out infeasible choices, disguised duplicates, and choices that violate stated constraints.

4. Keep the set selective.
- Default to 3 options.
- Use 2 when the choice is naturally binary or only two options are credible.
- Use up to 5 when the decision space genuinely benefits from more coverage.
- Exceed 5 only when the user asks for breadth or exhaustive ideation.
- Return fewer options rather than pad the set with weak ones.

5. Explain each option at the right depth.
- For alternatives, state what changes, the main upside, the main cost or risk, and when it fits best.
- For candidates, state why each fits and any caveat that could affect selection.
- For next moves, state the action, expected value or learning, and its cost or reversibility.
- Keep comparable options parallel enough to scan without flattening meaningful differences.

6. Make the decision easier.
- Recommend one option when the known goal and constraints create a defensible winner.
- Explain the recommendation using the deciding tradeoff, not personal preference or generic best practice.
- When no option clearly wins, name the unresolved fork and give either one deciding question or the cheapest useful test.

## Guardrails

- Ground project or implementation options in current evidence rather than generic possibility.
- Do not present minor variations as distinct strategies.
- Do not manufacture false balance when one option is clearly stronger.
- Include the status quo only when it is a credible choice with a meaningful advantage.
- Separate facts, inferences, and assumptions when uncertainty affects the comparison.
- Do not expand options into full implementation plans unless the user asks.
- Do not edit files, send messages, or take other actions merely because an option is recommended.
- Use relevant domain skills or research when the choice requires specialized or current knowledge; this skill governs the quality of the option set, not the underlying domain expertise.

## Output Contract

Adapt the format to the task and keep it concise. By default, return:

1. `Decision`
- One sentence framing what is being chosen.

2. `Options`
- A short numbered list or table with 2 to 5 choices and their decisive tradeoffs.

3. `Recommendation`
- Name the strongest choice and why, or state the unresolved fork and the best way to resolve it.

For a lightweight suggestion request, skip formal headings when they would add more ceremony than clarity.
