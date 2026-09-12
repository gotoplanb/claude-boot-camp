# Never Automate Writes Into a Vendor's Managed Package — Their Upgrade Becomes Your Outage

**Source:** Dave's post [AI at Build Time, Determinism at Run Time](https://davestanton.com/blog/ai-at-build-time-determinism-at-run-time) (2026-09-12), on the constraint he'd carry to any brokerage.
**Type:** preference
**Verified:** `ran-it` — a standing constraint on a live six-month build against Salesforce with a managed package (Left Main) installed.
**Relevant to:** 6 (Claude + Salesforce), 4 (integrating), 3 (administering)

## The constraint

> **The portal never writes to the managed package.**
>
> *"A managed package is someone else's contract, and automating writes into it turns their upgrade into my outage. The portal owns its custom objects and stops there."*

The vendor's transaction records stay under human control. Your integration owns **its own objects** and treats the vendor's as read-only.

There's a second instance of the same boundary in that build, running the other direction: the **Management Review** state (Unconfirmed / Confirmed / Missing Something) lives in Salesforce only, and the portal *"never reads or writes"* it. Managers confirm work was genuinely done — disclosure delivered, signature collected — in the system of record, untouched by automation.

## Why it generalises past Salesforce

Substitute any schema you don't control: a vendor's API, a third-party package, an upstream service's data model. The property that bites is the same — **you're coupling to a contract someone else revises on their schedule.** They ship an upgrade; your writes break, or worse, keep succeeding against changed semantics.

Reading has a graceful failure mode. Writing doesn't: a broken read shows you stale or missing data, a broken write corrupts the vendor's records and takes their support contract with it.

## Why this matters *more* with AI-assisted development

This is an old rule, and AI raises the stakes rather than changing it.

**Automating the write used to be enough work to make you think about it.** Wiring into an unfamiliar managed object was a day of reading docs — friction that functioned as a design review. Now it's a prompt, and the friction is gone. The decision to couple into someone else's contract can be made in ten minutes by someone who never framed it as a decision.

That's card 044's point with a specific edge: same failure, faster and easier to reach. And it's card 023's shape — the durable fix isn't reminding yourself to be careful, it's a **boundary you don't cross**, stated once and cheap to check in review.

## The practical form

- Your integration gets **its own custom objects**. Write there.
- Read the vendor's objects freely.
- Where the vendor's records need to change, **a human changes them** — and if that feels like a gap, it's usually a human review checkpoint you'd have wanted anyway.

## Why this matters

For consultants and SIs this is the difference between a deliverable that survives the client's next package upgrade and one that becomes an emergency during it — an emergency you'll be blamed for, since the upgrade was routine and your integration is the new thing.

It's also a clean example of the kind of constraint worth writing into a project's `CLAUDE.md`: unambiguous, checkable in review, and exactly the sort of thing a model will otherwise do helpfully and wrongly.
