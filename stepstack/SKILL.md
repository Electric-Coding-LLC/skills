---
name: stepstack
description: Turn a task into an ordered list of terse steps, each five words or fewer, complete enough to execute from. Use when Chris says /stepstack, $stepstack, or "stepstack", or asks for "just the steps", a "step list", or "quick steps" for a task or a process.
---

# Stepstack

Turn the task into an ordered, flat, numbered list of steps. Plan only; change nothing.

The list is done when someone could follow it without inventing steps in between. Its length is information: a small task gets a few steps, a big one gets dozens. Brevity lives in the wording of each step, never in leaving steps out.

## What a step is

One action with a result you could check. "Set the type scale" is a step. "Polish the details" is not; it is several steps hiding behind a verb.

- Start with a concrete verb. Never refine, polish, iterate, improve, handle, or finalize. If a step needs one of those, split it.
- A step that could appear in any task's list is too vague to keep.
- Five words or fewer. A file path, command, or proper name counts as one word.
- Forks go in the list as a step starting with "Decide:". Steps after it are written for the likely branch, or the branches are listed in turn. "Decide:" is for real choices that change later steps, not for stating an assumption.
- The reading you did to write the list is not a step. Include a read only when the executor must do it before acting.

## Grounding

- Repo task: read enough of the codebase to name real files, commands, and scripts. Write nothing.
- Domain or process task: list what skilled practitioners actually do, at the granularity they work at. Not what an overview article says they do.
- A question ("what does X do to achieve Y?") is a process task: list the process.
- If the task's premise does not hold (the thing to rename does not exist, the page is already there), say so in one sentence above the list, then list the nearest real task.

## Output

Return only the list, as a plain numbered list, not fenced. Explain only if asked. Before returning, count the words in each step and split any that run over.

## Examples

Repo task: "Add dark mode to the marketing site"

1. Read `styles/tokens.css`
2. List every hardcoded color
3. Replace each with a token
4. Add dark values under `prefers-color-scheme`
5. Add `data-theme` override selectors
6. Decide: toggle or system only
7. Add toggle to `components/header.html`
8. Persist choice in `localStorage`
9. Run `bun run build`
10. Check contrast on both themes
11. Screenshot every page, both themes
12. Update `tests/site.test.ts`

Process task: "What does a professional designer do to make a world-class website?"

1. Clarify the business goal
2. Define success metrics upfront
3. Interview stakeholders and customers
4. Audit analytics and existing content
5. Study competitors and category norms
6. Name the primary audience
7. Write the positioning statement
8. Agree scope, constraints, timeline
9. Map the site structure
10. Chart key user journeys
11. Assign one job per page
12. Write real copy first
13. Write headlines before layouts
14. Collect references and inspiration
15. Explore three visual directions
16. Pick one, explain why
17. Set the grid and spacing
18. Choose typefaces and type scale
19. Build a contrast-checked palette
20. Define motion and interaction rules
21. Wireframe key pages lo-fi
22. Design mobile first
23. Design the hardest page first
24. Build components, not pages
25. Cover every state and edge
26. Test with long, ugly content
27. Prototype key interactions
28. Test with real users
29. Cut everything nonessential
30. Spec tokens, breakpoints, behaviors
31. Pair with developers during build
32. QA build against design
33. Check accessibility and performance
34. Test across devices and browsers
35. Launch, measure, iterate
