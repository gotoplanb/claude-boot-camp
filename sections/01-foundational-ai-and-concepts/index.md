---
title: "1. Foundational AI and Concepts"
order: 1
status: drafting
---

## Scope

The vocabulary and mental models everything else depends on. Enough grounding to make good decisions, not a machine-learning course.

**This section is deliberately the thinnest original material in the boot camp.** Most of what belongs in a foundations hour — what these models do, tokens, context windows, prompting technique, where models fail — is already covered well by [Anthropic Academy](https://anthropic.skilljar.com) and the official docs. Duplicating it would be worse than linking to it, and it would date faster.

So the hour is built as **curated pointers plus a short original spine.** The original part is the material that isn't covered elsewhere because it comes out of running this work: how to *choose* a model, what makes work safe to walk away from, and where intelligence belongs in a system.

Per [curriculum.md](../../curriculum.md): lecture beats may be pointers; **labs stay original.**

---

## What the cards revealed about this section

The cards filed under §1 do **not** form one arc. They split into two clusters:

- **Cluster A — architecture for autonomy** (015, 018, 020, 021, 022, 033, 035, 048). Genuinely foundational, genuinely original. Stays here, though much of it is taught in depth in §5.
- **Cluster B — identity, access, governance** (034, 042, 044, 045, 046, 047). Not foundations. **Moved to §3**, where it forms that section's core.

That reassignment is the main structural finding of the first drafting pass.

---

## Part 1 — Pointers (~20 min)

Assign as pre-work or walk through quickly. Do not rewrite.

| Beat | Where it's already covered | Note |
|---|---|---|
| What these models are and aren't | Anthropic Academy; official docs | Core literacy |
| Tokens, context windows, and why context runs out | Official docs | Pair with the one original claim below about relevance beating volume |
| Prompting technique | Anthropic Academy; prompt engineering docs | Well covered; nothing to add |
| Where models fail and what failure looks like | Academy + model cards | |
| Evaluation basics | Official docs | See gap 1 — the corpus has nothing here |
| The current model lineup | Official model docs | **Never hard-code this.** Stamp a verification date; it dates in weeks |

**Action needed:** these are named, not linked. Someone has to do a pass, pick the specific pages, and record a checked-on date. Tracked as boot camp issue #1.

---

## Part 2 — The original spine (~25 min)

Four claims that aren't in anyone else's foundations course.

**1. Choose models by supervision shape, not benchmark rank (card 018, `ran-it`).**
The useful axes are *attended vs. unattended* and *fast-and-broad vs. slow-and-precise* — not which model scores highest. Sonnet for conversation because it makes unfiltered speech cheap; Opus for attended precision; Haiku for asynchronous, well-specified work. Capability ranking is the wrong instrument, and this is the most immediately practical thing in the hour.

**2. Model disposition drifts between versions, so re-check your instructions (card 015, `ran-it`).**
Context and instructions are tuned to a model's behaviour. After an upgrade, some of your carefully worded guidance is fixing *yesterday's* problem — and it's now just noise competing for attention. This is the foundations-level version of §5's "instructions lose to structure."

**3. You can walk away when something other than you can verify the work (card 020).**
Tests, builds, a diff against a golden file. If the deliverable is judgment, there's no external verifier and you'll supervise — plan for it rather than discovering it. `ran-it` as an observation; the generalisation is inferred, and carries `[confirm-this: run the Python-prototype → Fable-ports-to-Rust task and see whether the test suite really does substitute for watching.]`

**4. Spend intelligence at build time (card 048, `ran-it`).**
*If you can state the rule, encode it. If stating the rule is the hard part, that's where the model belongs.* Taught properly in §5.1; introduced here because it's the single decision that most changes what a system costs to run.

Optional fifth, if the audience is technical: **determinism buys debuggability, not correctness** (card 033, `inferred`).

---

## What's verified and what isn't

| Claim | Card | Status |
|---|---|---|
| Interface tier should match trust tier | 061 | `inferred` — single observed demo, no independent coverage |
| Automating a mediocre workflow makes it conspicuous | 061 | `inferred` — a reframe, not a measurement |
| Most ideas should die before being built | 058 | `ran-it`; the ~95% is Dave's estimate, **not** a measurement |
| Judgment is durable; mechanism is perishable | 072 | `ran-it` for one instance; generalisation `inferred` |
| Model choice by supervision shape | 018 | `ran-it` — daily practice |
| Disposition drifts across versions | 015 | `ran-it` |
| Build-time vs. run-time economics | 048 | `ran-it` — built and shipped |
| External verifier enables unattended work | 020 | `ran-it` observation, `inferred` generalisation |
| Determinism buys debuggability | 033 | `inferred` — reasoned, not measured |
| Cost shape changes behaviour | 035 | `inferred` — a bet; `[confirm-this: no published number for where a heavy MCP-over-chat workload lands against a plan's rolling cap.]` |

---

## Gaps — where more time is needed

**1. Evaluation is absent, and it's the one every CoE lead asks about.** The corpus is full of "how to know if it worked" at the task level — verify, observe, external signals — and has nothing on measuring whether a Claude deployment is delivering value against its cost. This is the biggest foundations gap, and it's unlikely to be fixable by linking out, because the interesting version is organisation-specific.

**2. No concrete cost model.** Cards 035 and 048 give principles for *thinking* about cost. Neither gives a CoE lead a number to take to a CFO. Even a rough shape — expect roughly $X per engaged user per month for exploratory work, $Y per call in production — would close it.

**3. No versioning or deprecation story.** Card 015 says disposition drifts; nothing says how long you can count on a model being available, or what the upgrade cadence does to a production system. Stakeholders ask this before they fund anything.

**4. Security is unaddressed as a topic.** The corpus covers identity and access well (now in §3) but never asks whether Claude introduces new attack surface — prompt injection against write-capable tools, data exfiltration through output. Card 044's *"ordinary architecture, just faster"* gestures at it without answering it.

**5. No capability-control policy.** Card 018 is a personal model-selection heuristic. Turning it into something a hundred people follow — routing policy, defaults, guardrails — is uncarded, and without it the cost argument in §3 doesn't hold.

**6. The pointer list itself needs building.** Part 1 names topics, not URLs. Until someone picks the pages and records a checked-on date, this half of the hour doesn't exist.
