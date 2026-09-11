# Default to Claude Code and Local Tooling — Building the Tool Is How the Thinking Happens

**Source:** Dave, Claude Boot Camp planning session 2026-09-11: "my default working style is to get everything going in Claude Code and use local tooling because I'm just a developer at heart and building the tooling is just how my brain naturally works."
**Type:** preference
**Verified:** ran-it — this is the observed default across the book, this website, the figure pipeline, and the boot camp itself.
**Relevant to:** 5 (building with Claude Code), general

## Content

The reflex on any new project is: open Claude Code, work against a local repo, build whatever tooling the problem needs. Not because it's optimal for every task, but because constructing the tool *is* the thinking process — the build clarifies the problem.

Evidence of the pattern in practice: a local conda environment and a least-cost-distance pipeline to draw one book figure; an MCP server to expose personal content; a card corpus with a custom verification schema. In each case the tool was built rather than adopted.

## The honest tradeoffs

**What it buys.** Everything is versioned, inspectable, reproducible, and yours. Local tooling has no vendor dependency and no per-seat cost. The artifacts compound — this project's card method came directly from the book's.

**What it costs.** The build reflex can fire when a hosted feature would have done the job in a tenth of the time. Claude Projects is a live example: Dave hadn't tried it, partly because "upload files to a web UI" doesn't look like tooling. Sometimes the right answer is the product that already exists.

**The tell** that this preference is misfiring: you're building infrastructure whose only user is you, for a problem you haven't confirmed you have. (The counter-discipline is already in `CLAUDE.md` — don't build the pipeline until the manual version proves annoying.)

## Why this matters

For the curriculum, this is a preference that has to be *labelled* as one. A reader who is not a developer at heart should not be told to build local tooling — the same job is often better done inside a hosted product, and the material should say so plainly rather than universalizing one temperament.

It also sets up the honest comparison the boot camp should make in several sections: *here is the build-it-yourself path, here is the hosted path, here's how to tell which one your situation wants.* That comparison is only credible if the author's own bias is stated out loud.
