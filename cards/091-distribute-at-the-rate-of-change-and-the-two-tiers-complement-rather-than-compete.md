# Distribute at the Rate of Change — and the Two Tiers Complement Rather Than Compete

**Source:** Dave, Claude Boot Camp session 2026-11-01, on why the Jira-recipe plugins specifically have to live in the repo. The rate-of-change framing and the coupling argument are Beta's, same session.
**Type:** concept
**Verified:** `inferred`. The axis is reasoned, not measured, and the engagement is not in production — *"recipes adjusted every couple of weeks at slowest"* is **Dave's estimate of a system that doesn't exist yet**, not an observed rate. The mechanism underneath (repo-committed skills load on checkout) is still `docs` — card 073's open question. *[confirm-this: once recipes are live, record the actual edit frequency. If it turns out to be monthly rather than fortnightly, the argument weakens and the org channel becomes viable.]*
**Relevant to:** 5 (building with Claude Code), 4 (integrating), 3 (administering), general

## The axis

> **Distribute at the rate of change. Match the channel's clock to the artifact's clock.**

That's a third axis for the question cards 073 and 090 keep circling, and it's the one that actually predicts:

| | Enterprise-distributed plugin | Repo-level plugin |
|---|---|---|
| Suits | Stable, org-wide, independent of any one codebase | Fast-moving, coupled to the code it describes |
| Examples | Organizational knowledge, departmental mandates and priorities, how decks get made | Recipes for Jira ticket types, dependency ordering, templates |
| Update mechanism | Central governance | `git pull` |
| Slow, governed channel is | **A feature** | A bottleneck |

A centrally governed channel isn't a weakness that repo distribution fixes. For a departmental standard, governance and slow change are exactly what you want — you do *not* want the org's deck conventions changing because someone pushed to main.

## The coupling argument is stronger than the speed one

Speed is the obvious reason and it's the weaker one. The real argument:

> A recipe depends on **the current metadata, the current CLI behaviour, and the current ticket templates**. It is only correct *as a set*.

Ship them in one commit and they stay consistent by construction. Put the recipes on an org-level channel and you've put a recipe and the code it describes on **different release clocks** — which is card 085's drift problem, manufactured deliberately, inside your own tooling.

So the test isn't only "how often does this change?" It's **"does this have to change at the same moment as something else?"** A thing that must move in lockstep with the code belongs with the code, regardless of how often it moves.

## The tiers shouldn't conflict

They're complements, not alternatives — which is a correction to how cards 073 and 090 frame it, where you pick a channel and that's the decision.

**Org-level plugins carry the customer's general standards. Repo-level recipes sit on top and stay specific to the work.** Both load. The repo layer should extend the org layer rather than restate or contradict it — a recipe that quietly reimplements a departmental standard is a divergence waiting to be discovered.

## One consequence of fast-moving recipes

**Pin the recipe version when a ticket is *claimed*, not when it's built.** If a recipe changes mid-run, a half-finished ticket shouldn't switch to different instructions partway through. Card 086 owns the stamping detail.

## Why this matters

It replaces "which channel is better" with a question that has an answer: *what is this thing's clock, and what does it have to stay in step with?* Stable and standalone goes to the governed channel. Fast-moving and coupled goes with the code.

And it rescues the governed channel from sounding like the bad option. Most of what an enterprise distributes genuinely should be slow.

See card 073 (the audience axis), card 090 (the velocity argument and the floor/ceiling structure), card 086 (version stamping and the recipe unit), card 085 (the drift that separate release clocks manufacture), and card 056 (conventions outliving the constraints that justified them — re-check this one when the edit rate is known).
