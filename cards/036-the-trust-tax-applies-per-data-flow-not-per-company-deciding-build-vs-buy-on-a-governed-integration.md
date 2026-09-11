# The Trust Tax Applies Per Data Flow, Not Per Company — Deciding Build vs. Buy on a Governed Integration

**Source:** Dave's prediction that Salesforce will meter MCP usage behind its trust-boundary narrative, Claude Boot Camp session 2026-09-11. The per-data-flow sharpening and the FLS correction are Beta's, same session. Pricing verified against [Agentforce pricing](https://www.salesforce.com/agentforce/pricing/) and the [Flex Credits rate card](https://www.salesforce.com/en-us/wp-content/uploads/sites/4/assets/pdf/agentforce/Flex-Credits-Rate-Card-04.21.2026.pdf).
**Type:** concept
**Verified:** mixed, deliberately — see the table below. The central **rule** is `inferred`; the pricing mechanics are `docs`; the prediction is explicitly a prediction. `[confirm-this: resolves when Salesforce in Claude pricing publishes — card 034.]`
**Relevant to:** 6 (Claude + Salesforce), 3 (administering — governance and cost), 4 (integrating)

## What's verified and what isn't

This card mixes a checked fact, a reasonable guess, and an outright bet. They're separated so nobody has to guess which is which:

| Claim | Status |
|---|---|
| Agentforce meters per action: 20 Flex Credits = **$0.10/action**, credits sold at **$500 per 100,000** (Voice actions 30 credits) | **`docs`** — current rate card, and rate cards get revised; re-check the date |
| Trust-layer machinery (masking, AI audit logging, retention guarantees) is *bundled into* that metered price rather than sold separately | **`inferred`** — consistent with how Agentforce is packaged, but not something I could confirm in the pricing docs |
| **Salesforce in Claude will also be metered** rather than riding free on a Claude subscription | **prediction** — Dave's, stated as a bet, not a claim. Pricing is unpublished (card 034) |

The reasoning behind the bet: Salesforce's trust-boundary story is a product position, not a courtesy. It costs them backend work to maintain, and the company's consistent pattern is to **meter through trust infrastructure rather than give it away.** A channel that has to satisfy the same trust narrative plausibly carries the same billing shape.

## What building your own MCP does *not* get you out of

Worth being precise, because the loose version of this argument is wrong and will get corrected in a procurement meeting:

**A custom MCP does not put you outside Salesforce's governance.** You're still hitting the REST/Bulk API under whatever your org edition licenses, and **field-level security and sharing rules are enforced server-side regardless of which client is asking.** That's not a loophole you'd want — it's the platform working correctly, and it's the same property that makes connector-backed artifacts safe (card 030).

## What the "tax" actually buys

The narrower and accurate version. You're opting out of an **additional AI-governance layer**, not out of Salesforce security:

- prompt-level masking before data leaves Salesforce
- audit logging of the AI interaction itself
- contractual retention guarantees with the model provider

That is a real product with real value — and it's value you may or may not need for a given integration.

## The rule

> **The trust tax applies per data flow, not per company.**

The decision variable isn't your industry. It's **what data specifically transits this integration, and whether that data's risk profile requires provable-at-audit AI governance** — as opposed to correct field-level security, which you get either way.

Consequences that a company-level heuristic misses:

- A **regulated** firm whose AI queries touch only product and inventory data may reasonably skip the tax **on that integration**, while paying it elsewhere for anything touching customer PII.
- A **small consumer-goods manufacturer** with no PII in the flow is paying for an assurance it can't use.
- The same company can correctly make **opposite** decisions on two integrations.

## Why this matters

It survives the question procurement will actually ask: *"why didn't you just buy the official one?"* — *"Because nothing in this data flow requires provable AI-interaction governance, and field-level security is enforced either way"* is a defensible answer. *"It seemed expensive"* is not.

It also generalises past Salesforce. Any platform selling a governed AI channel is selling assurance about the *interaction*, layered on access control you already have. The question is always the same: which of those two am I actually buying, and does this particular flow need the first one?
