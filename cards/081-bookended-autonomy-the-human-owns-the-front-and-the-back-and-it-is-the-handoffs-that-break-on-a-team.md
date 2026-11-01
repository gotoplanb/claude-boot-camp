# Bookended Autonomy — the Human Owns the Front and the Back, and It's the Handoffs That Break on a Team

**Source:** Dave, working notes for a Salesforce automation engagement, 2026-11-01. The solo half describes how he already works; the team half is forward-looking analysis of what that engagement will need.
**Type:** pattern
**Verified:** `ran-it` for the solo model — it's the shape of cards 001 and 058 stated as one rule. `inferred` for everything about two or more people: **no team has run this yet**, and the failure modes below are predicted, not observed. *[confirm-this: run one engagement with two or more people sharing a backlog and record which of the four breakages actually happen, and which turn out to be imaginary.]*
**Relevant to:** 5 (building with Claude Code), 6 (Claude + Salesforce), general

## The model

**The human owns the front and the back. The agent owns the middle.**

| | Who | What |
|---|---|---|
| Front | Human | Groom the backlog. Decide what's worth doing at all. |
| Middle | Agent | Implement, and do most of the verification. |
| Back | Human | Final acceptance. |

That's the whole shape. It generalizes cards 001 (explore in one session, build in another) and 058 (most ideas should die at the front) into a single division of labour.

## Why it's cheap solo

The grooming conversation carries all the intent, and **the same head that formed the intent does the acceptance check.** Nothing has to be written down for the handoff to work, because there is no handoff — the context never leaves the room.

That's worth naming explicitly, because it's the thing that silently stops being true.

## What breaks at two or more people

The method transfers. The *implicit context* doesn't:

- **Decisions made in one person's session are invisible.** If grooming happened in a chat and the conclusion never reached the backlog, it didn't happen as far as anyone else is concerned.
- **Two sessions can claim the same item**, or touch the same files. Work needs an explicit claim convention (card 083).
- **The reviewer wasn't at the grooming.** Someone reviewing an agent-built PR has none of the intent. Both the intent *and the verification evidence* have to travel with the PR — see card 088 for what that evidence looks like.
- **Shared convention files become load-bearing.** `CLAUDE.md`, recipes, and templates are what keep independent sessions consistent. Solo they're a convenience; on a team they're the only thing holding the line.

## The takeaway

> **The backlog and the PR have to carry the context that one person's head carries today.**

Which means the cost of going from one person to several isn't in the agent's work at all. It's in making the front and back handoffs explicit — and that cost is incurred *before* the second person arrives, or it's incurred as rework afterwards.

See card 001 (the two-session split this generalizes), card 058 (what the front of the pipeline is actually for), card 083 (claiming, and the parallelism that forces it), card 088 (what the back handoff needs to carry), and card 020 (whether the middle can run unattended at all).
