# skills

Agent skills shared by Claude Code and Codex.

Both agents load them through per-skill symlinks to this checkout:

```sh
mkdir -p ~/.claude/skills ~/.codex/skills
for d in */; do
  ln -sfn "$PWD/${d%/}" ~/.claude/skills/"${d%/}"
  ln -sfn "$PWD/${d%/}" ~/.codex/skills/"${d%/}"
done
find -L ~/.claude/skills ~/.codex/skills -maxdepth 1 -type l -delete
```

The last line removes links left behind by deleted skills.

A skill holds what an agent cannot infer: Chris's preferences, process facts, and pointers to Chris's own tools and references. Keep each one short and written for whichever agent reads it.
