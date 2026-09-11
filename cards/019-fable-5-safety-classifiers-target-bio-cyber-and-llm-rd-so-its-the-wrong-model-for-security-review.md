# Fable 5's Safety Classifiers Target Bio, Cyber, and LLM R&D — So It's the Wrong Model for Security Review

**Source:** Beta raised this in the 2026-09-11 session; verified against the Claude API reference (model catalog, Fable 5 migration section, refusal `stop_details` categories) in the same session.
**Type:** product-behavior
**Verified:** docs — read in the official API reference. **Not yet reproduced** against Dave's own security-review workload. `[confirm-this: run a representative security-review prompt on Fable and record whether it refuses, and with which stop_details.category.]`
**Relevant to:** 1 (foundational concepts), 3 (administering — model policy), 4 (integrating — API behaviour)

## The correction

Beta's framing was that Fable sits at the same tier as **Mythos** with extra safety layered on top. Half right:

- **Mythos is real.** `claude-mythos-5` exists, with the same capabilities, pricing, and API surface as `claude-fable-5`. The difference is **access, not safety** — Mythos is available only through Project Glasswing.
- **The safety behaviour is not a Fable-vs-Mythos difference.** The docs state that everything in the Fable 5 section applies to both models. So "Fable = Mythos + safety" is wrong; they're the same model surface with different availability.

What *is* true, and matters more: **Fable 5 runs safety classifiers on incoming requests, and cybersecurity is one of the targets.**

## The behaviour

Fable 5 runs classifiers targeting research biology and most cybersecurity content — the docs say plainly that Fable 5 **is not intended for those domains**. Refusal categories surfaced in `stop_details.category` include `cyber`, `bio`, `reasoning_extraction`, and `frontier_llm` (the LLM R&D case).

Mechanically, a decline is **not an HTTP error**: it returns a successful **200** with `stop_reason: "refusal"`. Code that reads `response.content[0]` without checking `stop_reason` first will break — pre-output refusals return an empty `content` array.

- A **pre-output** refusal is not billed at all.
- A **mid-stream** refusal bills the already-streamed partial — discard it rather than treating it as complete.

Recovery is **opt-in** on the API. Consumer surfaces ship with Opus 4.8 fallbacks built in, but an API request that doesn't ask for one simply stops.

## The consequence for this workflow

Dave **explicitly reserves Fable for security reviews** (along with heavy refactors touching lots of test coverage — that second use is unaffected by any of this). That's the one domain the classifiers are explicitly aimed at, and the docs note that benign adjacent work (security tooling, life-sciences tasks) does produce false positives.

So this isn't "Fable is a bit cautious here." It's **the wrong model for that job.** Opus 4.8 is the better fit for security review, and it's also the designated fallback target.

If Fable is used anyway for adjacent work, opt into fallbacks rather than letting a refusal fail the run:

```python
response = client.beta.messages.create(
    model="claude-fable-5",
    max_tokens=16000,
    betas=["server-side-fallback-2026-06-01"],
    fallbacks=[{"model": "claude-opus-4-8"}],
    messages=[...],
)
if response.stop_reason == "refusal":   # whole chain declined
    ...
```

## Two other Fable constraints worth knowing

- **Requires 30-day data retention.** Fable 5 is unavailable under zero data retention — an org configured below 30 days gets a `400` on *every* Fable request, however valid the payload. A confusing failure if you don't know to check.
- **The raw chain of thought is never returned.** Only a summary, and only if `display: "summarized"` is set (the default is `omitted`, which streams empty thinking blocks). Auditing a Fable run means reading the **tool-call trail**, not its reasoning — which is what Dave already does, but worth being explicit that the reasoning isn't available even if wanted.

## Why this matters

This is the failure mode the `Verified` field exists for: a plausible, confidently-stated model claim that was half-right, where the wrong half pointed at the exact task Dave uses that model for. Checking cost one lookup; not checking would have carded "Fable is just being cautious on security review" as fact.
