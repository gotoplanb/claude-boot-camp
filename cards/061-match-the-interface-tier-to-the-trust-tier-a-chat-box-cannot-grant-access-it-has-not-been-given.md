# Match the Interface Tier to the Trust Tier — a Chat Box Cannot Grant Access It Hasn't Been Given

**Source:** Dave watching the Dreamforce 2026 keynote live, 2026-09-15, and the Beta session that followed. The demo: a chat agent called **Piper**, with **Siemens** as the case study. A recipient of a partner-recruitment email follows the link, asks the chat agent how they might become a partner, and gets back "you're a good fit because…" — personalization driven by an **email lookup against Siemens' own Salesforce** (contacts, accounts, opportunities).
**Type:** concept
**Verified:** `inferred` — the reasoning is sound but rests on a single observed demo. The demo description is **Dave's live account only**; neither session had independent coverage of it, and no facts were added to what he saw. *[confirm-this: vendor attribution for "Piper" was not verified, and the lookup mechanism is Dave's read of the demo rather than a documented architecture.]*
**Relevant to:** 6 (Claude + Salesforce), 4 (integrating), 3 (administering — governance), 1 (foundational concepts)

## The principle

**How much interface you give someone should track how much trust you've established with them.** The two ends:

| Trust tier | Right surface | Why |
|---|---|---|
| Anonymous / unverified visitor | A screening surface. A qualifier. | No authenticated identity, so there is nothing real to grant. Shallow is the *ceiling*, not a design failure. |
| Verified, credentialed partner | An actual tool with programmatic access | They've already earned the access a chat box was never able to hand out. |

The Siemens demo sits at the easy end. An anonymous visitor with no auth is exactly the case where screening is all the interaction *can* be — so the chat agent isn't underachieving, it's doing the most its trust tier permits.

## The sharper failure isn't the AI

The instinct is to call this an AI problem — "prove you're valuable before we'll engage" feels newly cold when a bot says it.

It isn't new. That's the same friction as a gated "request a demo" form with mandatory qualification fields, which B2B has run forever. Wrapping it in chat doesn't make it worse *in kind*. It makes it worse *in visibility*: a form reads as paperwork, while a chat reads as a conversation you're being screened out of in real time.

Which relocates the actual lesson:

> **Automating a mediocre workflow with flashy new tooling doesn't fix the workflow. It makes the mediocrity more conspicuous.**

The qualification gate was always the questionable part. AI didn't introduce it; it removed the paperwork alibi that used to hide it.

Dave's reputational-harm instinct is picking up something real, but it's aimed one notch off. The harm isn't "AI qualified me." It's "you spent your novelty budget making the screening feel like a conversation instead of making the relationship worth having."

## Why the low-trust version is the one that gets demoed

Worth naming, because it's the unflattering and probably correct explanation: a public chat widget doesn't require solving partner identity first.

The valuable interactions — deal registration, co-sell support — need the vendor to hand a partner **real programmatic access to their Salesforce data, on that partner's behalf**. That's not a UI decision. It's an identity and authorization problem, the same access-bundle/credential shape as card 037's Claude Tag provisioning, aimed at external partners instead of internal staff.

So the keynote-friendly demo and the actually-useful capability aren't two points on a product roadmap. They're separated by infrastructure nobody had to build for the demo.

## Why this matters

It gives you a question to ask of any "we added an AI assistant" proposal: **what trust tier is the user in, and what could this surface actually grant them?** If the honest answer is "nothing, it can only screen," then a conversational interface oversells it — and overselling a screening gate is how you get card-carrying reputational damage from a feature that technically worked.

It also predicts where the real work is. Anything past screening is gated on identity, not interface.

See card 062 (the partner-ecosystem version of the high-trust end), card 037 (admin-provisioned access bundles — access and instructions travelling together), card 039 (what actually bounds the blast radius once you *do* grant access), card 032 (pre-built views don't scale to ad-hoc questions — the other half of why shallow chat disappoints), and card 044 (none of this is AI-specific).
