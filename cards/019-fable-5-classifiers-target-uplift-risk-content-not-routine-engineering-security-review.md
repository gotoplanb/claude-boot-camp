# Fable 5's Classifiers Target Uplift-Risk Content, Not Routine Engineering Security Review

**Source:** Beta raised the safety-layer question in the 2026-09-11 session. Mechanics checked against Anthropic's bundled `claude-api` skill reference (model catalog, Fable 5 section, `stop_details` refusal categories) — a **cached** reference, not a live page fetch. Canonical sources to re-check against:
[models overview](https://platform.claude.com/docs/en/about-claude/models/overview.md) · [introducing Claude Fable 5](https://platform.claude.com/docs/en/about-claude/models/introducing-claude-fable-5.md) · [migration guide](https://platform.claude.com/docs/en/about-claude/models/migration-guide.md) · [API errors](https://platform.claude.com/docs/en/api/errors.md).
**Corrected the same day** by Dave's own experience — this card initially overclaimed.
**Type:** product-behavior
**Verified:** `docs` for the mechanics (official API reference); `ran-it` for the practical finding that routine security review on Fable does **not** trip the classifiers — Dave runs security reviews on Fable and has seen no noticeable refusals.
**Relevant to:** 1 (foundational concepts), 3 (administering — model policy), 4 (integrating — API behaviour)

## The correction to *this card*

An earlier version of this card concluded Fable was "the wrong model for security review." **That was wrong, and it's a useful wrong.**

It was reasoned from a docs phrase — Fable 5's classifiers target "most cybersecurity content," and it "is not intended for those domains" — without distinguishing what that phrase actually covers. It covers **uplift-risk content**: material that would meaningfully advance offensive capability. It does *not* cover a developer reviewing their own codebase, triaging SonarQube findings, or auditing test coverage.

Dave runs exactly that kind of review on Fable and sees no noticeable refusals. A `ran-it` observation beat a `docs` inference, which is the schema working as intended.

## The correction to the Mythos framing

Beta's original claim was that Fable sits at the same tier as **Mythos** with extra safety layered on top. Half right:

- **Mythos is real.** `claude-mythos-5` exists with the same capabilities, pricing, and API surface as `claude-fable-5`. The difference is **access** — Project Glasswing only.
- **Safety is not the Fable-vs-Mythos difference.** The docs state the Fable 5 section applies to both models. "Fable = Mythos + safety" is wrong.

## What the classifiers actually do

Fable 5 runs safety classifiers on incoming requests, targeting research biology and offensive-security content. Refusal categories surfaced in `stop_details.category` include `cyber`, `bio`, `reasoning_extraction`, and `frontier_llm` (the LLM R&D case). The docs do note that benign *adjacent* work — security tooling, life-sciences tasks — can produce occasional false positives, so the failure mode is real but narrow.

Mechanically, a decline is **not an HTTP error**: a successful **200** with `stop_reason: "refusal"`. Code reading `response.content[0]` without checking `stop_reason` first will break — pre-output refusals return an empty `content` array.

- **Pre-output** refusal: not billed at all.
- **Mid-stream** refusal: the already-streamed partial *is* billed — discard it rather than treating it as complete.

Recovery is **opt-in** on the API. Consumer surfaces ship with Opus 4.8 fallbacks; an API request that doesn't ask for one simply stops. If you're doing security-adjacent work at volume, opt in rather than letting an occasional false positive fail a run:

```python
response = client.beta.messages.create(
    model="claude-fable-5",
    max_tokens=16000,
    betas=["server-side-fallback-2026-06-01"],
    fallbacks=[{"model": "claude-opus-4-8"}],
    messages=[...],
)
if response.stop_reason == "refusal":   # the whole chain declined
    ...
```

## Two other Fable constraints worth knowing

- **Requires 30-day data retention.** Unavailable under zero data retention — an org configured below 30 days gets a `400` on *every* Fable request, however valid the payload. Confusing if you don't know to check.
- **The raw chain of thought is never returned.** Only a summary, and only with `display: "summarized"` (the default is `omitted`, which streams empty thinking blocks). Auditing a Fable run means reading the **tool-call trail**, not its reasoning.

## Why this matters

Two lessons, and the second is the bigger one.

The narrow lesson: "cybersecurity content" in a safety-policy sentence means uplift risk, not the everyday engineering sense of the phrase. Reading policy language with a developer's definitions produces confidently wrong conclusions.

The broader lesson: this card is what a corpus is supposed to do to itself. A `docs`-verified inference met a `ran-it` observation and lost, within a day, because both were labelled and therefore comparable. Had the original claim been written as unmarked prose in a section draft, it would have read as authoritative and quietly steered someone away from a model that works fine for the job.
