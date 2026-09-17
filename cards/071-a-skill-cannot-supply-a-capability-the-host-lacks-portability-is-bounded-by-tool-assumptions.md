# A Skill Can't Supply a Capability the Host Lacks — Portability Is Bounded by Tool Assumptions

**Source:** Beta, Claude Boot Camp session 2026-09-17, ruling screenshots out of the release-plugin lab (card 070) and naming the general reason.
**Type:** concept
**Verified:** `inferred` — follows from what a skill is (card 069) plus the surface differences already established in card 038. Not tested by authoring one plugin and running it across surfaces. *[confirm-this: run the same skill from Claude for Mac, Claude Code and an API-driven context and record what degrades.]*
**Relevant to:** 4 (integrating), 2 (operating Claude products), 5 (building with Claude Code), 1 (foundational concepts)

## The principle

**A skill is instructions. It can tell Claude what to do; it cannot give Claude the ability to do it.**

Anything that *acts* — taking a screenshot, querying a warehouse, reading a file — needs a tool the **host environment** provides. Card 069 makes this point about connectors; it generalizes to every tool:

> Skills are portable. Tool assumptions are not.

The same skill file behaves differently depending on where it runs:

| Surface | A "take a screenshot" instruction |
|---|---|
| Claude for Mac with computer use | Executes |
| Claude in Chrome | Executes, within the browser |
| Claude Code | Nothing to execute against |
| API-driven / unattended | Nothing to execute against |

Nothing about the skill changed. The host did.

## Why this bites specifically on plugins

A plugin bundles skills for distribution (card 069), which means it's **authored on one surface and installed on others**. Whatever tools were ambient when you wrote it are not guaranteed wherever it lands.

So a plugin that quietly assumes computer use is a plugin that half-works for anyone who installs it elsewhere — and the failure isn't loud. The skill loads, Claude reads an instruction it can't carry out, and what you get is an apology or an improvised substitute rather than an error.

This is card 038's shape at a different layer. There, the surface determined which MCP servers were *reachable*; here it determines which tools are *present*. Same lesson: **the surface is part of the environment, and a bundle that ignores it is a bundle that travels badly.**

## What to do about it

**Decide the tool dependency deliberately, at authoring time.** Two honest options:

1. **Declare it.** The plugin requires computer use, and says so. Fine when you control where it's installed.
2. **Degrade gracefully.** The skill handles everything that needs no tool and *reminds the human* to do the rest. This is the choice card 070 makes for screenshots — the release skill formats notes, changelogs and version conventions, and tells you to grab the screenshots yourself.

Option 2 keeps the artifact portable, and the reminder is not a consolation prize: a checklist item a human executes is more reliable than a tool call that may not exist.

**The test when authoring:** for each instruction, ask *what has to be attached for this to run?* If the answer isn't "nothing," that's a dependency, and it needs one of the two treatments above.

## Why this matters

It's the counterweight to the thing that makes skills appealing. They're cheap, portable, and plain text — so it's easy to write one as though text were sufficient, and describe an action the model has no way to perform.

The distinction to hold: **a skill changes what Claude knows to do; a tool changes what Claude can do.** Bundling the first has never supplied the second.

See card 069 (skill/command/connector/plugin — connectors as the specific case of this), card 038 (hosted surfaces reach remote MCP servers only — the same surface-dependence one layer down), card 070 (where this ruling gets applied), and card 020 (unattended contexts, which have the fewest tools attached and hit this first).
