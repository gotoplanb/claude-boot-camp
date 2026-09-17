# Lab Design — the Scheduled Morning Brief, and Why the Schedule (Not the Chat) Is the Point

**Source:** Dave, Claude Boot Camp session 2026-09-17, proposing a scheduled-task lab. The scope split and the schedule-vs-chat distinction are Beta's, same session.
**Type:** decision
**Verified:** `n/a` for the design. **The lab is not yet buildable** — see the gate below. Filed as a card so the design survives until the prerequisites are run.
**Relevant to:** general (curriculum structure), 2 (operating Claude products), 4 (integrating)

## The lab — scheduled morning brief

A single unattended run that exercises Calendar, Gmail and Drive together:

1. **Trigger on a schedule**, early morning.
2. **Read the calendar** for today's events.
3. **For each event, identify the attendees** — who the invites are from and to.
4. **Search Gmail for mail involving those people.**
5. **Summarize** what's relevant to that meeting.
6. **Create a Google Doc per meeting** containing the summary.

One pass, three services, no human present. That's the whole core lab.

## The line worth teaching — the schedule and the chat are not two versions of one thing

This is the part that earns the lab its place, and it's easy to blur.

You could just *ask* Claude for the same summary in a chat. Pleasant, and it requires **neither a schedule nor a connector** — so if the lab presents "ask Claude live" as the easy alternative, students learn nothing about why any of this machinery exists.

> **The scheduled task's job is to make the Drive doc exist and be populated *before you would ever think to ask*.**

Nothing runs on its own without it. The live-chat path only becomes interesting once you want the answer already sitting somewhere — for you later, or for someone else at all. Drive isn't a nicer output surface here; it's the **persistence** that makes an unattended run worth doing.

Drawing that line explicitly tells students what the schedule buys, which is the actual lesson. The Doc is good enough as the persistence layer for the lab.

## Scope: core lab vs. takeaway

**Core lab** — the six steps above. Buildable, testable, demonstrably unattended.

**Takeaway / extension** — stated, not solved:

- Updating the same doc afterwards with what came *out* of the meeting (voice capture closing the loop)
- The genuinely harder **"which doc is this"** matching problem that update implies
- "Ask Claude live" as an alternate front end onto the same stored data

Keeping these out is deliberate. They're the interesting-but-messy half, and card 020's rule applies — an unattended lab should demonstrate one thing that works, not three that might.

## The lab should teach card 067 in passing

A scheduled task writing to Drive is exactly where **account drift** would bite (card 067). The lab should have the task **assert which account it's on before writing**, and fail loudly if it's wrong.

That's one extra step, and it converts a lurking gotcha into a demonstrated practice: verify the *input* context, not just the output.

## The gate — what must be run before this becomes a lab

`labs/README.md`: *"A lab may only be built from cards marked `ran-it`."* This one isn't there yet. Outstanding:

| Question | Currently |
|---|---|
| Can a scheduled task use connectors at all? | Assumed in card 066, never verified |
| Does a schedule resolve the connector at fire time, or pin it at creation? | Card 067's decisive `confirm-this` |
| Can a scheduled task **write** to Drive (create a Doc), not just read? | Unverified |
| Does Gmail search from an unattended run behave like it does in chat? | Unverified |

The first three are one sitting's work and would upgrade cards 066 and 067 at the same time. Until they're run, this is a design, not a lab.

## Why this matters

It's a clean answer to "what is a scheduled task actually *for*," which is a question the boot camp will get and which demos usually answer badly by showing something you could have asked for directly.

It also models the curriculum's own discipline: the design gets filed immediately so it isn't lost, and the lab waits on execution. Writing this up as a runnable lab today would mean teaching four unverified steps from the front of a room — the exact failure `labs/README.md` exists to prevent.

See card 049 (case studies beside labs), cards 066 and 067 (the connector behaviour this depends on), card 020 (unattended work needs a verifier), and card 005 (a way, not the way — the takeaway exists so participants find their own).
