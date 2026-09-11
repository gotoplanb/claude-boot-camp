# Cost *Shape* Changes How People Use the Tool — Subscription Limits vs. Per-Token Metering

**Source:** Beta, Claude Boot Camp session 2026-09-11, refining an argument that chatting with Salesforce data via a Claude subscription is cheaper than routing it through metered enterprise AI.
**Type:** concept
**Verified:** `inferred` — the behavioural claim is reasoned, not measured. The mechanics (subscription plans use rolling usage limits rather than per-token billing) are `ran-it` from ordinary use. `[confirm-this: no published number for where a heavy MCP-over-chat workload lands against a plan's rolling cap.]`
**Relevant to:** 3 (administering — cost), 1 (foundational concepts), 6 (Claude + Salesforce)

## Two cost shapes

| | **Subscription** (Pro/Max/Team) | **Per-token metering** (API, consumption credits) |
|---|---|---|
| You pay | A fixed amount, monthly | Per unit of work |
| Constraint | A **rolling usage limit** — a cap over a several-hour window | Your budget |
| Marginal cost of one more question | **Effectively zero** until you hit the cap | Real, and visible |
| Behaviour it produces | Explore freely; ask the dumb question | Ration; batch; hesitate |

**"As much as you want" is not literal.** Subscription plans carry rolling limits, not infinity. But the *shape* is what matters: below the cap, asking one more question costs nothing you can feel.

## Why the shape matters more than the price

The same total spend produces **different behaviour** depending on how it's metered.

Per-token pricing makes every question a small decision. People batch, hesitate, and skip the exploratory follow-up — which is exactly the interaction the tool is best at (card 018: Sonnet is chosen because it makes unfiltered rambling cheap). Metering doesn't just cost money; it **suppresses the usage pattern that creates the value.**

That's a governance point, not an accounting one. A center-of-excellence lead choosing a metered surface for exploratory work has quietly decided that people will explore less, and won't see that decision in a budget line.

## Where a flat-rate argument actually holds

The "chat with your data via your existing plan" argument is strongest in the **DIY case** and for a specific reason: **you're already paying for both sides regardless.** The Claude subscription and the Salesforce API access exist whether or not you use them together — so the marginal cost of the pattern is genuinely near zero.

That argument does **not** transfer automatically to a prebuilt product. The moment a supported option has its own pricing (card 034 — Salesforce in Claude's pricing is unpublished), the comparison has to be redone against whatever it actually costs. Flat-rate-beats-metered is a claim about *two specific things you already own*, not a general law.

## Why this matters

It gives a defensible reason to put exploratory work on subscription surfaces and production/automated work on metered ones — which happens to match the split in card 018 (Haiku via API for well-specified async work; chat on a plan for thinking out loud).

And it's the honest version of a claim people overstate. "It's unlimited" is wrong and gets corrected embarrassingly; "the marginal cost of one more question is effectively zero, up to a rolling cap" is right, survives scrutiny, and makes the same point.
