# One CLI Is One Chokepoint — Which Makes It the Place to Emit Telemetry

**Source:** Dave, working notes for a Salesforce automation engagement, 2026-11-01.
**Type:** pattern
**Verified:** `ran-it` for the OTel practice generally — Watchtower is a live local stack and card 022's feedback-loop discipline comes from it. `inferred` for this specific design: **the CLI described here hasn't been built**, so the in-memory-exporter testing approach and the durable run log are proposed rather than observed.
**Relevant to:** 5 (building with Claude Code), 4 (integrating), 3 (administering — governance), 6 (Claude + Salesforce)

## The argument

Routing all orchestration through one CLI (card 087) buys something beyond tidiness: **a single chokepoint where every step of every run can emit structured telemetry.**

That's a free consequence of the architecture, and it's worth taking deliberately rather than discovering later. If orchestration is scattered across shell snippets, prompts and a Makefile, there is no such point and instrumenting after the fact means instrumenting everywhere.

## The design

- **Emit OpenTelemetry**, or structured JSON. Vendor-neutral, configured by the standard environment variables — so where the data goes is a deployment decision, not a code change.
- **Tests use an in-memory exporter and assert on spans.** That's card 063's ordering discipline arriving in a different domain: if the sequence is the property you care about, assert on the sequence.
- **A developer's own machine exports to a local stack.** Same emission, different sink.
- **The customer default is a durable run log** — not a backend they have to buy.

## The trap: in-memory telemetry dies with the session

Worth stating on its own, because it's the failure that makes unattended work unauditable:

> Spans held in memory vanish when the session ends. An unattended run (card 084) therefore needs **a small durable record** — steps, commands, results, timings — attached to the PR or the ticket.

Without it, the runs you most need to reconstruct are exactly the ones nobody was watching. This is card 039's *log what the calls did, separately from what the model said*, applied to a pipeline instead of a session.

## Don't sell a backend by default

The consulting stance, and it's the part most likely to be got wrong:

Start from discovery questions, not a recommendation. **What do you use today? Who would actually look at run data? What can't you see right now?** Then recommend — and **disclose your familiarity with anything you name.** Someone recommending the stack they happen to know should say that's what they're doing.

Pushing a paid observability product into an engagement that asked for Salesforce automation is how a useful idea becomes a sales motion, and the customer can tell.

## Why this matters

Observability is usually retrofitted, badly, after something goes wrong unattended. Here the structural decision (one CLI) and the observability decision are the same decision, taken once, early, at no extra cost — which is the only version of this that actually happens.

See card 087 (the CLI that is the chokepoint), card 078 (Claude Code's own OTel export — the adjacent signal), card 022 (build the feedback loop first), card 039 (log what the calls did, not what the model said), card 084 (why unattended runs need a durable record), and card 005 (selling nothing — the stance behind the last section).
