# A Harvested API Sequence Is a Hypothesis, Not an Equivalent — ERP UIs Fire Logic the API Doesn't

**Source:** Beta, Claude Boot Camp session 2026-09-16, flagging the load-bearing assumption in card 063's harvest pattern when the target is an SAP-style ERP.
**Type:** gotcha
**Verified:** `inferred` — a known-shape risk in ERP-style systems, reasoned rather than observed. **Nobody in this session has hit it.** *[confirm-this: run a harvested sequence against a real SAP sandbox across a range of inputs and record whether any UI-path validation or workflow step fails to fire. Until then this is a caution, not a finding.]*
**Relevant to:** 4 (integrating), 6 (Claude + Salesforce), 5 (building with Claude Code), 3 (administering)

## The assumption being made

Card 063's method rests on one thing being true: that **the API surface is a faithful 1:1 mirror of what the UI transaction does.**

In SAP-style systems, it frequently isn't.

A transaction screen commonly triggers work that the equivalent BAPI/RFC call does not:

- **Validation** that lives in the screen's flow rather than in the callable interface
- **Workflow and approval routing** kicked off as a side effect of the UI path
- **Side-effecting business logic** that is either absent from the API or present but requiring an *explicit separate step* the UI invokes implicitly

## Why your verification pass won't catch it

This is the nasty part. The harvest method uses the UI as the oracle — click through, confirm the end state looks right.

But **the end state can look identical and still be missing something.** A skipped validation or an unrouted approval doesn't necessarily change the record you're looking at. It changes what happens later, or what happens to an input your sandbox run never exercised.

So the check that makes the pattern feel safe is exactly the check that is blind to this failure mode.

## What to do instead

Treat the harvested script as a **hypothesis to load-test across a range of inputs**, not a confirmed-equivalent replacement — specifically the first time you run this against an unfamiliar ERP-style backend.

Concretely, that means:

- Exercise **edge-case inputs**, not just the happy path you harvested from
- Check for downstream artifacts — approval items, workflow instances, audit entries — rather than only the target record
- Where the vendor documents a BAPI as requiring a separate commit or validation call, assume the UI was doing it for you
- Scale is a revealer: something that looks fine on one supplier can be visibly wrong across a thousand

## Why this matters

It's a scoping rule for card 063, not a refutation of it. The harvest pattern is right; the confidence it produces is calibrated to systems where the API is the real interface and the UI is a client over it.

Enterprise ERPs often invert that — the screen *is* the business logic, and the API is a partial export of it. Knowing which kind of system you're in is the difference between "I codified the process" and "I codified the visible half of the process."

The general form, worth carrying beyond SAP: **an automation derived from observing a UI inherits only the behavior the UI exposed to you.** Anything the UI did on your behalf, silently, is exactly what you won't know to reproduce.

See card 063 (the pattern this bounds), card 024 (Salesforce: "created" does not mean "usable" — the same shape of invisible-gap failure), card 022 (observe reality, not the report — and here, be precise about *what* reality you observed), and card 026 (why this gets discovered in a sandbox and not in production).
