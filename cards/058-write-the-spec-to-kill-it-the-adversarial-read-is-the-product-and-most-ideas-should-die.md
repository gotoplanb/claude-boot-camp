# Write the Spec to *Kill* It — the Adversarial Read Is the Product, and Most Ideas Should Die

**Source:** Dave, Claude Boot Camp session 2026-09-14, reflecting on the session written up in [Two Hours to Arrive at `cp`](https://davestanton.com/blog/two-hours-to-arrive-at-cp) — a full architecture spec for a secrets-management proxy that ended with the decision to keep copying `.env` files by hand.
**Type:** preference
**Verified:** `ran-it` — the standing working method, with the linked session as the worked example. The ~95% figure is Dave's own estimate of his kill rate, not a measurement.
**Relevant to:** general, 1 (foundational concepts), 5 (building with Claude Code)

## The practice

Write the spec. Hand it to Claude. **Ask to be challenged, and mean it.** Then expect the idea to die, and count that as the session working rather than failing.

The worked example: a reasoning proxy in front of Doppler — reason codes matched against policy, approval queues, asymmetric model tiering with the strongest model on the defensive side, OpenTelemetry spans on every access event. Architecture diagram. Build order. ECS deployment topology. Two hours later the conclusion was `cp` — copy `.env` files out of iCloud Drive, which was already the existing behavior.

> *"I didn't pay two hours for `cp`. I paid two hours to not spend three days building a FastAPI service, a policy engine, an approval queue, and an ECS deployment for a problem I don't have."*

## The emotional default is the load-bearing part

The technique is easy and the stance is not. Asking to be challenged does nothing if you arrive attached to the answer — you get the challenge, defend the spec, and ship it anyway.

So the default posture going in is: **I am thinking out loud about something I am going to throw away.**

- Most ideas are bad. That's the normal case, not a failure of the idea-having.
- Ideas are now cheap enough that having a bad one costs nothing — *provided* you don't build it.
- Sunk cost is the whole enemy, and it accrues to the spec too, not just to code. Two hours of design is enough to start defending something.
- Excitement two hours ago is not evidence. *"You have to be willing to throw away the thing you were excited about two hours ago. That's the whole skill."*

## Don't scope it perfectly up front — start, and let it evolve

The corollary, and the reason this is a mindset shift rather than a tip: **the attempt to scope something perfectly before starting is the failure, not the cure for it.**

Precision up front feels like rigor. It is mostly a way of committing before you know anything. The spec's job is not to be right — it is to be *specific enough to attack*. Vague ideas can't be argued with; that's what makes them survive longer than they deserve.

Which reframes what the spec is for. It isn't a plan. It's an argument device — the artifact that makes a real critique possible, and therefore disposable by design.

## What the failed session still produced

Killing the idea is not the same as ending with nothing. The two hours left behind:

- **Two reusable ideas** — a judge that may only move a decision in the restrictive direction (so poisoning it buys nothing), and asserting *event ordering* in tests rather than outcomes.
- **A named conflation** — two projects wearing one name. The real secrets problem was already solved by a folder; the interesting part was a security lab that has no business guarding live credentials.
- **A shelved project, honestly shelved** — with a stated test for whether to revive it: *is this a friction point in my life today?*

The instinct to salvage an idea by building a smaller version of it is the same sunk-cost move in a cheaper suit. Shelving is a real outcome.

## Why this matters

This is one of the core mindset shifts of working with Claude, and it is not about prompting.

When the cost of *articulating* an idea collapses, the bottleneck moves from having ideas to **discarding them fast enough**. A model that will argue back is the cheapest filter available for that — and the only thing that makes it work is being genuinely willing to lose the argument.

The corpus already contains the structural version of this move (card 054 — MIT-0 as a commitment device that forecloses premature generalization; card 023 — when a behavior keeps needing willpower, change the situation). This card is the *behavioral* one it depends on: the willingness to be wrong out loud, early, before anything is built.

See also card 001 (the exploratory chat is where this happens — not in the repo-writing session), card 009 (building the tool is how the thinking happens — the counterweight that keeps this from becoming an excuse to build nothing), and card 020 (an external verifier standing in for you — here the verifier is checking the *premise*, not the output).
