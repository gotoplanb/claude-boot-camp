# Group Open Questions by Blocker — the Count of `confirm-this` Flags Overstates the Work

**Source:** Dave, Claude Boot Camp session 2026-09-12, reading the first drafting pass: *"you answered one of your own questions by saying I need to use Claude Tag."*
**Type:** pattern
**Verified:** `ran-it` — done once against this corpus at 56 cards. 26 `confirm-this` flags plus 5 open issues grouped down to roughly 6 experiments.
**Relevant to:** general (operating a card corpus), 2 (operating Claude products)

## The situation

Cards get filed one at a time, and each one records its own uncertainty in its own `[confirm-this: ...]` flag. That's correct at filing time — the flag belongs with the claim it qualifies, and the filer has no idea what the twentieth card will need.

The side effect shows up later. At 56 cards this corpus carried **26 open flags and 5 open issues**, which reads as thirty-one outstanding threads. It isn't. Grouped by *what would actually resolve them*, it's about **six**:

| Blocker | Resolves |
|---|---|
| Stand up a Team/Enterprise tenant and run Claude Tag + Cowork | cards 037, 046; issues #6, #7; part of 007 |
| Publish artifacts, incl. connector-backed, with a second viewer | cards 027, 028, 029, 030 |
| Build one hosted Salesforce MCP connector | cards 030, 031, 032 |
| Run one weeks-long session through real compactions | cards 008, 017 |
| Small standalone trials (a few hours each) | cards 003, 006, 012, 016, 020 |
| Not actionable — waiting on someone else | cards 002, 013, 034, 035, 036, 055 |

Four to six separate threads dissolve into *"go use Claude Tag for an afternoon."*

## Why it matters

**Thirty-one open questions reads as a corpus in trouble. Six experiments reads as a Tuesday.** Same information, and the second one gets acted on.

Two failure modes the grouping prevents:

- **Paralysis by inventory.** A long flag list looks like debt, and debt that large invites doing none of it. The grouped list has an obvious first move.
- **Doing the same setup five times.** Without grouping you resolve card 037 one week and card 046 the next, standing up the same tenant twice, because the flags sit in different files and nothing connects them.

It also surfaces the third category, which is the quietly valuable one: **flags nobody can resolve.** Waiting on Salesforce to publish pricing (034, 036) or on a lawyer (013) isn't work — it's a watch item. Mixing those into the same list as runnable experiments makes the backlog look bigger and more stuck than it is.

## How to do it

Periodically — after a drafting pass, or whenever the flag count starts feeling heavy — read every open flag **at once** and sort by *what single action would close it*, not by card, section, or age.

Three buckets fall out: **one setup unlocks a cluster**, **standalone and cheap**, **blocked on someone else**. Rank the first bucket by cluster size, because that's leverage.

Cheap to do, and it can only be done by someone holding the whole corpus at once — which is the argument for having a librarian rather than letting each card manage its own uncertainty.

## The general shape

Any system that records uncertainty locally will overstate it globally. Bug trackers do this, research backlogs do this, `TODO` comments do this.

The count is an artifact of *where the notes get written*, not a measure of remaining work. **Re-derive the real number by asking what resolves things, not by counting what's open.**
