# When a Skill Isn't Doing What You Wrote — Reload Seams and Precedence That Runs Backwards

**Source:** Beta, Claude Boot Camp session 2026-09-18, flagging two mechanics that aren't visible from outside.
**Type:** gotcha
**Verified:** `docs` — Beta's research pass; **neither was reproduced here.** *[confirm-this: edit a plugin's `.mcp.json` mid-session and confirm it needs `/reload-plugins`.]* *[confirm-this: create the same skill name at project and personal level and observe which wins — this one is counterintuitive enough that it should be seen, not assumed.]*
**Relevant to:** 5 (building with Claude Code), 2 (operating Claude products), 4 (integrating)

## The shared symptom

*"I changed the skill and nothing happened"* and *"the skill is behaving like a version I didn't write"* look identical from the chair. Two different mechanisms produce them, and neither is guessable.

## 1. Live-reload has a seam

Reloading is not uniform across the things that live in a skill folder:

| What changed | To take effect |
|---|---|
| The `SKILL.md` file itself | **Automatic** — picked up mid-session |
| A plugin's `hooks/`, `.mcp.json`, `agents/` | **`/reload-plugins`** |
| A brand-new top-level skills *directory* | **Full restart** |

So a folder that is *both* a skill and a plugin reloads **half automatically**. Edit the instructions and the change lands; edit the `.mcp.json` beside it and it doesn't — with no error, because nothing failed.

The debugging trap: you conclude the edit was wrong and start rewriting content that was fine. Before changing anything twice, ask *which reload mechanism does this file use?*

## 2. Precedence runs backwards from the guess

When the same skill name exists at multiple levels:

> **enterprise → overrides → personal → overrides → project**

**The narrowest scope loses.** Most people guess the opposite, because narrower-wins is how CSS, shell `PATH` shadowing, and local config overrides generally behave — the specific thing beats the general one. Here the general thing wins.

The consequence lands on **repo-committed skills** (card 073) hardest: they're project-level, which is the bottom of the stack. A teammate with a personal skill of the same name silently runs theirs instead of the repo's, and neither of you gets told.

**Plugin skills dodge this entirely** — they're namespaced `plugin-name:skill-name`, which is the `data:` prefix from card 069. A namespace can't collide with a bare name.

**The mitigation:** give repo-level skills **distinctive names**. Not `review`, `deploy`, `test` — those are exactly the names someone already has personally.

## Why this matters

Both failures are silent, and silence is the expensive property. Neither produces an error, so the time cost isn't the fix — it's the stretch of debugging spent on the wrong layer, editing content when the problem was a reload mechanism, or rewriting a skill that was never the one running.

Two questions worth asking before touching the content:

1. *Which reload path does the file I changed use?*
2. *Am I sure the skill that ran is the file I'm looking at?*

**Both claims here are unrun.** The precedence order especially — it's counterintuitive, which is exactly the condition under which a plausible-sounding recollection is most likely to be wrong. Verify before teaching it.

See card 073 (repo-root skills, which sit at the losing end of precedence), card 069 (namespacing), and card 014 (the general shape — believing a stale picture of the world rather than re-checking it).
