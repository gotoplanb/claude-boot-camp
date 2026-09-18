# Repo-Root Skills vs. a Plugin Marketplace — Audience Decides, and Most Skills Never Leave the Repo

**Source:** Beta, Claude Boot Camp session 2026-09-18, sharpening Dave's tooling hierarchy. Fills a tier card 060's distribution table skipped.
**Type:** decision
**Verified:** `n/a` for the stance. The supporting mechanics are `docs` — Beta's research, not run here. *[confirm-this: commit a skill to `.claude/skills/` in a repo, clone it elsewhere, and confirm it loads with no install step.]* *[confirm-this: the SKILL.md/Copilot claim below — find and link the primary source before repeating it.]*
**Relevant to:** 5 (building with Claude Code), 4 (integrating), 2 (operating Claude products), general

## The missing tier

Card 060 sorts distribution by the **Claude org boundary**: just you → your team's marketplace → outside the org entirely. That skipped a tier, and it's the one most skills belong in.

**Commit skills to `.claude/skills/` in the repo.** Anyone who clones it gets them. No install step, no marketplace, no plugin manifest — scoped exactly to *whoever works in this codebase*.

| Who needs it | Mechanism |
|---|---|
| Just you | A personal skill |
| **Anyone working in this repo** | **`.claude/skills/` committed to the repo** |
| People across repos, teams, the org | Plugin + marketplace |
| People outside your Claude org | Custom MCP server (card 060) |

## The rule

**The deciding factor is audience, not preference.**

> If the only people who'd ever need the skill are people already checking out that repo, repo-root skills are strictly simpler — **don't package a plugin at all.**

Save the plugin/marketplace step for skills that must reach beyond a single codebase. Packaging something that never leaves its repo buys you a manifest, a catalog, a publish step and an install step, in exchange for nothing — the clone already delivered it.

These are **two genuinely different mechanisms**, not two routes to the same place. Repo-root skills travel with the code and are versioned by it; a marketplace skill travels independently and has to be updated on its own schedule (card 059's propagation point).

## Why this is easy to get backwards

The plugin route is the one that *feels* like the real answer, because it has ceremony — a manifest, a catalog, an install command, something to show. The repo route is one directory and a commit, so it reads as the beginner option.

It isn't. It's the correctly-scoped option for the common case, and it has a property the marketplace can't match: **the skill and the code it operates on are versioned together.** A repo-root skill can never be stale relative to its codebase, because it *is* the codebase.

## One collision caveat

Repo-level skills lose name collisions to personal and enterprise ones (card 074). Give them **distinctive names** so a coincidental personal skill on someone's machine doesn't silently shadow yours.

## The ladder may extend past Claude

Worth tracking rather than relying on: the `SKILL.md` format is reportedly being picked up as an open convention beyond Claude — described as natively compatible with **GitHub Copilot Chat in VS Code**.

If that holds, the artifact you write is portable across vendors, not just across surfaces — which would make the format a better bet than any single product's packaging around it. **Flagged, not established:** this came from a research pass with no link captured, and it's the kind of claim that would be embarrassing to teach if the compatibility turns out partial. Find the source first.

## Why this matters

It changes the default. The question *"should I build a plugin?"* mostly answers **no** — the audience is usually people who already have the repo, and for them the cheapest correct mechanism is a committed directory.

See card 060 (the org-boundary axis this completes), card 074 (collisions and reload seams), card 070 (the release plugin — a case that *does* need to leave its repo), card 059 (context cost and update propagation), and card 069 (what a skill actually is).
