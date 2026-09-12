# Separate What Things *Cost* From What You *Absorb* — and Derive Consumption, Never Store It

**Source:** Dave's post [AI at Build Time, Determinism at Run Time](https://davestanton.com/blog/ai-at-build-time-determinism-at-run-time) (2026-09-12), on a billing rebuild that wasn't on the brief.
**Type:** pattern
**Verified:** `ran-it` — built, tested, and deliberately held out of production pending user-acceptance testing, with a production audit queued to run in report mode first.
**Relevant to:** 6 (Claude + Salesforce), 3 (administering), general

## Two books, not one

- **The price book** — what things cost. Moves when a vendor requotes.
- **The credits book** — what the organisation absorbs. Moves when partner compensation policy changes.

They change for **different reasons, on different schedules, owned by different people.** That's the test for whether two things are actually one thing.

> *"A product can have a price and no allowance, or an allowance and no price. Conflating them is what produced the duplicate columns."*

The symptom of the conflation was visible in the legacy system and had been for months: an accounting Print Log whose *"Total Charges (Marketing)"* and *"Total Charges (Agent)"* columns carried **identical numbers**. The format had two columns because the real process has allowances — and the system filled both with the same value, because it only ever modelled one concept.

**Duplicated values across columns that are supposed to differ is a modelling smell, not a display bug.** Someone built the shape of the right answer and then had nothing to put in it.

Allowances then come in two shapes, and both are needed: **counted** (50 colour sides per listing per month) and **dollar-denominated** (a $185 monthly postage budget).

## Derive consumption, never store it

> *"Consumption is derived, never stored — remaining balance is computed by summing what earlier requests covered, so there's no running total to drift out of step with the charges themselves."*

A stored running total is a second source of truth for something you can already compute. It will drift — on a failed write, a retried request, a cancellation, a manual correction — and when it does, nothing errors. The number is just quietly wrong, and it's the number people trust.

Deriving costs a query and removes an entire class of silent bug.

## What the rebuild surfaced

Four live defects, each quietly mischarging for months:

| Defect | Effect |
|---|---|
| Seeded placeholder prices outranking real ones | Buyer's guides billed at $120 instead of $15 |
| Postage priced in the catalogue but never charged | 65% under-charge on postcards |
| Flat-charge path ignoring quantity | 250 postcards billed at three cents total |
| Requote dropping postage | Silent under-charge on every revision |

> *"Volume doesn't create defects like these. It makes them expensive."*

There's a related timing fix worth stealing: **the charge lands when a request is marked complete, not when it's submitted.** That matched how the team already described their own process, and it fixed two bugs at once — a cancelled request used to stay charged, and a deliverable whose PDF failed to assemble used to charge for something never received.

## Why this belongs in a Claude curriculum

Because of what accelerates: **AI-assisted delivery brings the volume sooner.** These defects were latent for months and harmless while request counts were low. Ship a system that makes ordering ten times easier, and you convert a modelling shortcut into real money moving incorrectly — faster than anyone's review cadence.

So the advice from that build — *"fix billing before the volume arrives"* — is specifically a consequence of building quickly. The domain model you could previously get away with is the one your new throughput will expose.

Worth pairing with the release judgment from the same project: the rework is **held out of production pending UAT**, with an audit running in **report mode** before any correction. *"I'd rather know the size of the problem than discover it while fixing it."*
