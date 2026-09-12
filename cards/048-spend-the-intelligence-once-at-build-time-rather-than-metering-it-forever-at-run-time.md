# Spend the Intelligence Once at Build Time Rather Than Metering It Forever at Run Time

**Source:** Dave's post [AI at Build Time, Determinism at Run Time](https://davestanton.com/blog/ai-at-build-time-determinism-at-run-time) (2026-09-12), from six months of fractional CTO work at a real estate brokerage.
**Type:** concept
**Verified:** `ran-it` — built and shipped. 1,775 tests at a 92% branch-coverage floor.
**Relevant to:** 1 (foundational concepts), 3 (administering — cost), 5 (building with Claude Code), general

## The principle

> **The intelligence got spent once, at construction, instead of metered forever at execution.**

A frontier model built the thing. The thing doesn't call a model to run.

## The example that makes it concrete

An agent wants a flyer for a listing. A designer built the template months ago; the job is swapping five photos and a price into it.

**That is a deterministic transformation, not an AI problem.** Claude Code built the PDF region-mapping engine, the schema system, the billing logic, and the test suite. The running system calls **PyMuPDF**.

> *"There's a version of this project where every flyer generation is an LLM call, the demo is more impressive, and the brokerage has a four-figure monthly bill for string substitution. That version is worse in every way that matters."*

Note what that sentence concedes: the wasteful version **demos better**. That's why this decision gets made badly — the expensive architecture is more impressive in the room where it's chosen.

## Where the model did earn its place

Three features generate text — agent bios, listing descriptions, meeting agendas. All route through a dispatcher, **local model first on hardware already paid for**, cloud as fallback rather than default.

The line is card 033's, arriving from the cost side rather than the governance side:

| | **Build-time** | **Run-time** |
|---|---|---|
| The work is | A transformation — same input, same output | A judgment — genuinely different each time |
| Flyer example | Swap a photo and a price | Write a 220-character *Just Sold* line |
| Who does it at run time | A function | A model |
| Cost shape | Paid once | Paid per request, forever |

**The test:** if you can state the rule, encode the rule. If stating the rule *is* the hard part, that's where the model belongs.

## Two cost decisions worth stealing

From the same build, both non-obvious:

- **AI cost is an input cost the end user never sees** — like postage or electricity. *"An agent billed thirty-four cents for using a tool the brokerage asked them to use is a support call that costs more than the line item."*
- **Running out pauses the work rather than billing for it.** *"A ceiling that bills instead of stopping isn't a ceiling."* Distinguish allowances that permit overage from ones that refuse — and make the AI budget the refusing kind.

Also worth noting: the budget is metered **in dollars, not calls**, because a router forwarding a job to a paid model spends real money that a call-counter scores as free.

## Why this matters

It's the sharpest available answer to "where does AI belong in this system," and it inverts the default instinct. The reflex is to put the model in the *product*. The higher-leverage move is usually to put it in the *construction* — and then ship something with predictable cost, no latency, no refusals, and no dependency on a vendor's uptime.

It also pairs with card 035: build-time spend is a **one-off**, run-time spend is a **cost shape you're committing someone to indefinitely**. Choosing the second for work a function could do is how an AI project acquires a permanent bill for string substitution.
