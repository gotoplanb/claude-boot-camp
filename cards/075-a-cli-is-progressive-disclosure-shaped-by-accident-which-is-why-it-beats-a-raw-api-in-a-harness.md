# A CLI Is Progressive-Disclosure-Shaped by Accident — Which Is Why It Beats a Raw API in a Harness

**Source:** Beta, Claude Boot Camp session 2026-09-18, supplying the mechanism behind a preference card 063 states without explaining.
**Type:** concept
**Verified:** `ran-it` for the preference — card 063 is Dave's standing method. `inferred` for the mechanism as stated here; the two causes below are reasoned, not measured. *[confirm-this: compare token spend on the same task driven by a documented REST API vs. an equivalent CLI.]*
**Relevant to:** 5 (building with Claude Code), 4 (integrating), 1 (foundational concepts)

## The claim card 063 leaves unexplained

Card 063 says to target **CLI calls over API calls**, and gives a thin reason: it executes as one command. That's true and it isn't the interesting part.

The real mechanism is two things:

**1. A CLI's documentation is fetched on demand.** `--help` is there when wanted. An API surface has to be *front-loaded* — schemas, endpoints, field descriptions — before the model can use any of it.

**2. Claude already carries strong priors on CLI conventions.** Flags, subcommands, `--help`, exit codes, stdin/stdout. Those are learned from training, so a CLI arrives partly pre-understood in a way a bespoke API surface does not.

## Why this is the same idea one layer down

This is card 011's passive-vs-active axis, and card 059's cost of it, applied to a target system rather than to Claude's own extension mechanisms:

| | Front-loaded | On demand |
|---|---|---|
| Claude's mechanisms (card 059) | MCP tool schemas | Skills |
| A target system (this card) | A raw API surface | A CLI |

**A CLI is progressive-disclosure-shaped without anyone having designed it that way.** Unix conventions produced that property for entirely unrelated reasons — human discoverability at a terminal — and it turns out to be the same property that makes a surface cheap for a model to drive.

That's worth naming, because it means the efficiency isn't something you engineer. **You get it by choosing a target that already has the shape.**

## What follows

- **When both exist, prefer the CLI** — not as style, but because the help text is retrievable instead of resident.
- **When you're building the thing Claude will drive**, a CLI is the cheaper interface to offer, and you inherit the priors for free.
- **When only an API exists**, the front-loading cost is real and worth counting. Wrapping it in a thin CLI can pay for itself if the task recurs — which is exactly card 063's harvest, ending in a script.

## Why this matters

It converts a preference into a prediction. *"CLI beat API for us"* is an anecdote; *"a surface whose documentation is retrievable on demand costs less than one that must be resident"* tells you which way an unfamiliar case will go, and why wrapping an API can be worth the wrapper.

It also generalizes past CLIs: **ask of any integration whether its documentation has to be resident or can be fetched.** That single question predicts the context bill better than the size of the API does.

See card 063 (the method this explains), card 011 (passive vs. active retrieval), card 059 (the same tradeoff priced for skills and MCP), and card 071 (what a skill can reach once it's there).
