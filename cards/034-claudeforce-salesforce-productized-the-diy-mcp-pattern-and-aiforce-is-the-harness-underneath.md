# Claudeforce — Salesforce Productized the DIY MCP Pattern, and AIforce Is the Harness Underneath

**Source:** Raised by Beta in the Claude Boot Camp session of 2026-09-11 as a correction to a half-remembered product name. Verified against the [official Salesforce press release, 2026-08-26](https://www.salesforce.com/news/press-releases/2026/08/26/salesforce-and-anthropic-announce-claudeforce/).
**Type:** product-behavior
**Verified:** `docs` — quotes below are from the press release. Pricing claims are explicitly **not** verified; see the pricing note. `[confirm-this: re-check after open beta ships — this card describes a product mid-rollout and will age fast.]`
**Relevant to:** 6 (Claude + Salesforce), 3 (administering), 1 (foundational concepts)

## What it is

Announced **26 August 2026**. Two directions, which land in different places and are worth separating:

**1. Claude *into* Salesforce.** Claude as the reasoning model for Agentforce's Atlas Reasoning Engine, powering "Agentforce Vibes and Agentforce Coworker by default." Delivered "through Amazon Bedrock… **within the Salesforce Trust Boundary**, allowing customers, including those in regulated industries, to deploy domain-specific AI."

**2. Salesforce *into* Claude.** A plugin — **"Salesforce in Claude"** — with **37 prebuilt sales skills** (meeting prep, deal health review, pipeline review), letting sellers "reason over live revenue context, automate pipeline updates, and take governed action" from inside Claude's own interface. Onboarding "reads a seller's enterprise context" and stands up a tailored dashboard.

**Timeline:** pilot customers now; **open beta expected September 2026**; additional non-sales skills "will begin launching in late 2026."

## The part that matters most here

> **AIforce** — "Salesforce's trusted enterprise harness that brings all your business data and workflows to any agent through **MCP servers, APIs, and CLI tools**, without complicated and costly integrations."

That is a first-party, supported version of the pattern this boot camp has been building by hand: connect once, chat with your Salesforce data from Claude (card 032). Salesforce built the harness.

Two consequences:

- **The DIY MCP route is now the fallback, not the only road.** For a client, "use the supported plugin" is an easier sell than a bespoke integration — and easier to hand over when the engagement ends.
- **The DIY route keeps a real advantage**: it isn't limited to 37 sales-shaped skills, and it works today against any object and any question. Prebuilt skills are pre-built views again (card 032) — excellent for the anticipated cases, useless for the unanticipated ones.

Worth noting how much weight Salesforce is putting behind this: it has **never before attached its "force" suffix to another company's product.** *(That framing is from third-party commentary, not the release.)*

## Pricing — unknown, and don't guess

The release says nothing about pricing, and carries the disclaimer *"Pricing and packaging are subject to change."*

A reasonable **inference**, not a fact: Agentforce has historically billed on consumption-based Flex Credits, and nothing in the announcement suggests the embedded direction changes that. So heavy day-to-day use of an Agentforce-embedded assistant plausibly meters at enterprise scale.

Whether **Salesforce in Claude** rides on an existing Claude plan the way a connector would, or carries its own charge, is **unknown**. Do not assume either. Any cost comparison against the DIY pattern (card 035) has to be redone once that pricing is public.

## Why this matters

It changes what section 6 is teaching. "Here's how to wire Claude to Salesforce yourself" was the whole story a month ago; now it's one of two paths, and a consultant needs to know when each wins. The DIY route remains the answer for arbitrary questions and non-sales objects; the plugin is the answer for supported, governed, handover-friendly deployment of the common cases.

It's also a reminder that this curriculum has a **short half-life on product facts** — this one landed three weeks before the card was written, and the open beta may have shipped by the time anyone reads it.
