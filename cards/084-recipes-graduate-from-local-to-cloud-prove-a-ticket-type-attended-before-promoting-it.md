# Recipes Graduate From Local to Cloud — Prove a Ticket Type Attended Before Promoting It

**Source:** Dave, working notes for a Salesforce automation engagement, 2026-11-01.
**Type:** pattern
**Verified:** `inferred` — a proposed staged path, **not yet run on an engagement**. The criterion it rests on is `ran-it`: card 020's external-verifier test is observed. The graduation process built on top of it is design. *[confirm-this: take one ticket type — adding a field — through the whole ladder and record where it actually sticks.]*
**Relevant to:** 6 (Claude + Salesforce), 5 (building with Claude Code), 3 (administering)

## The shape

**Discover how a ticket type gets built and verified locally, with humans watching. Once it's proven, promote that type to unattended cloud execution.**

The unit being promoted is the **ticket type**, not the agent and not the project. "Add a field" graduates; "redesign the approval flow" never does. That distinction is what makes the ladder tractable — you're not deciding whether to trust automation, you're deciding which shapes of work have earned it.

This is card 063's harvest with a different payload: use the expensive attended run to learn the sequence, then ship the cheap repeatable version.

## Graduation criteria

A type promotes when all four hold:

1. **Verifiable without a human** — check-only deploy, tests, queries. This is card 020's diagnostic verbatim: *what tells me this succeeded, other than my own reading of it?* No answer means it stays local, permanently if necessary.
2. **No hidden prerequisites.** Anything a person did by hand in a UI becomes an explicit precondition, or the type doesn't graduate. This is where most candidates fail, and card 024 is why — *created* and *usable* are different states, and the gap is usually a click somebody forgot they made.
3. **Bounded blast radius, with easy rollback.**
4. **Several clean local runs first.** Not one.

## Start with the boring ones

Adding a field. Lookups. Cross-object formulas.

These are the right first candidates precisely because they're unglamorous: deterministic, checkable, reversible, and frequent enough that automating them pays. Resist starting with the interesting work — the interesting work is the work with judgment in it, which is the work that fails criterion 1.

## Why this helps the customer

It's **a staged path to trust rather than a day-one leap into autonomy.**

That's the part worth saying out loud in a client conversation. "Let the agent deploy to your org" is a request for trust nobody has grounds to give yet. "Here are four runs of this one ticket type, attended, with the verification output attached — can we run the fifth unattended?" is a request anyone can evaluate.

Each graduation is also evidence for the next one, which makes this the same move as card 058's security-ask: go with a specific, demonstrated, bounded request instead of a general one.

See card 020 (the verifier test that criterion 1 is), card 063 (learn expensively once, run cheaply forever), card 024 (why criterion 2 catches most candidates), card 026 (where the local runs happen), and card 082 (what "promote to cloud" means concretely).
