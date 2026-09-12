# MIT-0 by Default on Tools — the License as a Commitment Device Against Premature Generalization

**Source:** Dave, Claude Boot Camp session 2026-09-12, on why most of his repositories are public and default to MIT-0.
**Type:** preference
**Verified:** `ran-it` — the standing default across his public repositories.
**Relevant to:** general, 5 (building with Claude Code)

## The practice

Most repositories public. Default licence **MIT-0** — MIT with the attribution requirement removed, so a user owes nothing at all: no credit, no notice, no conditions.

This applies to **tools**, and is a different choice from the one made for **writing** (card 013 — the card corpus is CC BY-SA, which *does* require attribution and keeps derivatives open). The asymmetry is deliberate, and the reason is at the bottom of this card.

## The stated reasons

- **Saying up front that you can just use this.** Not an intention to hoard.
- **It's replicable anyway.** With current tools, anyone who genuinely wanted to rebuild these could.
- **No appetite for the commercial motion.** Selling a tool means inside sales and lead generation. *"We need people that do that, but that ain't me."* Not a claim that selling is wrong — several of these could plausibly be SaaS — just not the work he wants.

Those are all true and none of them is the interesting one.

## The actual mechanism

> **It provides a psychological constraint** — and it works *even though he believes nobody will use these projects except him.*

The licence isn't primarily addressed to users. It's addressed to the author.

Stamping MIT-0 on a repo at the start forecloses a future that would otherwise sit in the back of your mind: *maybe this becomes a product.* And that latent possibility is expensive, because it quietly changes what you build:

- generalising for users who don't exist
- adding configuration for cases nobody has asked for
- polishing surfaces nobody will see
- treating the tool as the point, rather than the problem in front of you

> *"I'm building a tool to solve the problem that I have in front of me. I'm not building the tool for the tool's sake."*

Declaring the tool free removes the reason to do any of that. What's left is the only thing that was ever paying: **solving your own problem faster.**

## Why it's the same move as everything else in this corpus

This is card 023's principle turned on the author instead of the model: **when a behaviour keeps needing willpower, change the situation instead.**

You could try to remember not to over-generalise. That's a standing vigilance instruction, and it loses — the pull toward "but what if someone else wants…" arrives fresh on every design decision. A licence file is a **constraint that doesn't argue**: cheap, permanent, decided once, and visible in the repo forever.

It's also why the asymmetry with card 013 isn't inconsistent. **CC BY-SA on the writing is a choice about how ideas propagate; MIT-0 on the tools is a choice to stop thinking about it.** Different instruments because they're solving different problems — one is about terms, the other is about attention.

## Why this matters

Premature generalisation is among the most expensive failure modes in tool-building, and it's almost always justified by a hypothetical future user. The justification is hard to argue with in the moment precisely because that user can't be disproven.

Removing the commercial future removes the argument. And it does so at the cheapest possible moment — before the first commit, when the file costs nothing to add and there's no sunk investment defending it.
