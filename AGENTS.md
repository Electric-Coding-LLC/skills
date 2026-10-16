# skills

Skills shared by Claude Code and Codex. Each top-level folder is one skill, linked into `~/.claude/skills` and `~/.codex/skills` from this checkout (see `README.md`).

Write skills for whichever agent reads them: name a tool-specific feature only with "if the app has it". Keep them short and limited to what an agent can't infer.

## Delivery

- **Gate:** every top-level folder has a `SKILL.md` whose frontmatter has `name` (matching the folder) and `description`.
- **While iterating:** edits are live immediately through the symlinks; start a fresh session to see them.
- **Review focus:** wording that only works in one agent, procedure the model already knows, rules that over-constrain, overlap between skills, anything private (this repo is public).
- **Sensitive areas** (always a full review): `deliver/`, since it authorizes merges and go-live.
- **Merge:** edits to skills, this file or the README commit straight to `main` after the gate; a change to `deliver/` goes through a squash PR. No required checks.
- **Go live:** after merge, `git switch main && git pull --ff-only` in `~/dev/electric/skills`.
- **Verify live:** the checkout is on `main` at the merge commit, and every link in `~/.claude/skills` and `~/.codex/skills` resolves into it.
- **Deploy policy:** automatic
