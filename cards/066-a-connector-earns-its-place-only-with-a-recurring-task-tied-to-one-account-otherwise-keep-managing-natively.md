# A Connector Earns Its Place Only With a Recurring Task Tied to One Account — Otherwise Keep Managing Natively

**Source:** Dave, Claude Boot Camp session 2026-09-17, deciding which of several Google accounts (if any) was worth connecting.
**Type:** preference
**Verified:** `ran-it` — the Bosshardt Drive case is a live connection; the decision not to connect the other accounts is the standing choice.
**Relevant to:** 2 (operating Claude products), 4 (integrating), general

## The test

A connector pays for itself when there's a **specific, recurring, reference-or-update workflow tied to a single account**.

Not "keep an eye on my inbox." General triage is exactly the case connectors serve worst — it's open-ended, it's the thing you already have good native tools for, and it doesn't have a shape the connector can be scoped to.

**The case that works:** Drive connected to a client account (Bosshardt) where weekly updates get uploaded and shared documentation gets referenced. Narrow, repeated, and tied to one account — so Gmail and Calendar stay off and only Drive is on.

**A scheduled task answers the test by definition.** If you're setting something up to fire on a schedule — the scheduled-task surface in Claude for Mac, say — it *is* a recurring use case, so the "is this worth connecting" question is already settled the moment you start scheduling. And it's not optional: a task that fires on its own and needs to read or write somewhere needs access that's live and authenticated **at fire time**, because there's no one there to swap accounts or intervene. Scheduling is the case where a connector stops being a convenience and becomes the channel the work runs through. It also raises the stakes on card 065's one-account limit considerably — see card 067.

**Business and team contexts fit naturally**, because the single-account model from card 065 matches how the org actually works. A small team sharing one workspace has *one* relevant account to connect, not five personal ones to juggle. The constraint that's friction for an individual is a non-issue there.

## When to skip it

**You're already comfortable managing several inboxes natively.** If mail on your phone works fine and no repeated task needs Claude reading or acting on that data inline in a chat, the connector adds little — and the one-account limit becomes friction rather than something you'd never notice.

**You already have a programmatic path to the same data.** If your normal working pattern for an account is Claude Code plus a CLI (`gcloud` and friends), the chat-based Drive connector is **redundant for that account** — you have a lower-friction, more scriptable route to the same files. Adding the connector doesn't extend reach; it duplicates it worse.

That second one generalizes: **a connector competes with whatever access path you already have, and often loses.** Ask what it lets you do that your current path doesn't, and if the answer is "the same thing, in a chat," skip it.

## Don't build a bridge to fix friction you don't have

The tempting move once card 065's limit bites is a custom multi-account MCP server.

**Don't, unless simultaneous multi-account access is genuinely the requirement.** Swapping accounts takes about a minute and costs nothing but visibility (card 065), so the bar for building infrastructure is: *do I need these accounts connected at the same time, repeatedly?* If the honest answer is "no, I just found the swap annoying," that's friction, not a gap.

This is card 060's trap in a smaller frame — taking on an ongoing engineering obligation to avoid a recurring inconvenience — and card 058's discipline about ideas that shouldn't be built. A real multi-account need is a deliberate infrastructure decision. Irritation is not.

## Rule of thumb

| Situation | Call |
|---|---|
| One account, one clear recurring use case | Connect it, and scope narrowly — turn off the services you don't need |
| Scheduling an unattended task against that data | Already answered — connect it, and pin the account (card 067) |
| Several scattered accounts, no single recurring Claude-in-the-loop task | Skip it; keep managing natively |
| Already have a CLI/programmatic path to that data | Redundant for that account |
| Genuinely need simultaneous same-provider multi-account access | A real gap — custom MCP or third-party bridge, treated as infrastructure, not a default |

## Why this matters

Connectors present as obviously-good — more context, more capability — so the default drift is to connect everything and let Claude sort it out. That's the wrong default: it maximizes exposure (card 046 — Claude acts with *your* permissions) and context cost (card 059) in exchange for capability you may not use.

The discipline is the same one that runs through the corpus: attach the narrowest thing that serves an actual repeated task, and leave the rest disconnected.

See card 065 (the account limit this decision lives inside), card 046 (whose permissions you're extending), card 059 (what a connected surface costs in context), card 060 (build-vs-reach), and card 012 (a curated Project upload — often the cheaper way to get Claude the context you wanted).
