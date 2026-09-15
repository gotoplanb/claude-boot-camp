# Distribute Skills to a Verified Partner Network — Deal Registration and Co-Sell Are the Motions Worth Building

**Source:** Dave, reacting to the Siemens/Piper partner-recruitment demo in the Dreamforce 2026 keynote, 2026-09-15: *"That seems like just a tremendously more valuable than just a better chat bot."*
**Type:** preference
**Verified:** `inferred` — an untested product idea Dave likes the shape of, not something built or observed. The mechanism it depends on (skills distributed to people outside your Claude org) is real; this specific application is a bet. *[confirm-this: is there a supported path for publicly distributing skills to an external audience you don't administer, and what identity does the partner authenticate with?]*
**Relevant to:** 4 (integrating — MCP and APIs), 6 (Claude + Salesforce), 3 (administering — governance), 2 (operating Claude products)

## The idea

A vendor with a partner ecosystem — Siemens in the observed case — **publishes skills to its verified partner network**. A partner adds them to their own Claude. The ongoing partner relationship then runs through that channel rather than through a web form or a portal login.

The motions that would actually earn it:

- **Deal registration** — submit a deal, get it acknowledged and deduplicated against the vendor's pipeline.
- **Co-sell support requests** — ask for a sales engineer, pricing approval, or joint-call support.
- **Co-support escalation** — the same shape for post-sale issues.

These are the interactions partners genuinely have with vendors, repeatedly, and they are currently some of the worst software in the enterprise world.

> *"I'm not going to just go to a website and type into a chat box."*

That objection is the whole argument. A partner is already working somewhere — their own tools, their own Claude. Asking them to travel to your site and start a conversation is asking them to leave their context to do your paperwork.

## Why this is the high-trust end, structurally

This is card 061's principle at the far end of the spectrum. A partner is **verified and credentialed**, so there's something real to grant — and a chat widget was never able to grant it anyway.

It's also a clean instance of card 060's dividing line: partners are **outside your Claude org** by definition. You will never administer their tenant, so org-internal distribution (plugin marketplaces, admin-provisioned bundles) can't reach them. This is the case custom distribution exists for — reach, not cost.

## The prerequisite everyone skips

**Partner identity is the hard part, and it comes first.**

Deal registration means a partner's Claude acting against the vendor's Salesforce, on that partner's behalf, scoped so Partner A cannot see Partner B's pipeline. That is:

- an authorization boundary per partner org (cards 039, 042)
- delivered through something that can enforce it, not just hold a key (card 043 — OAuth turning an MCP server into a policy enforcement point)
- with the credential arriving alongside instructions for using it (card 037's pairing)

Which is why the chat widget got the keynote slot and this didn't. **The demo-friendly version is the one that doesn't require solving partner identity.** Any vendor proposing this should be asked for the identity model before the interface mockups.

## Why this matters

Partner ecosystems are a large, badly-served surface where the incumbent tooling is portals nobody wants to log into. The distribution mechanism now exists to do better, and the bottleneck has moved to authorization design rather than interface design.

It's also a useful test for the whole "AI for partners" category: if a proposal's most valuable interaction still requires the partner to come to *your* site, it hasn't used the new mechanism at all — it's a portal with a nicer input box.

See card 061 (interface tier vs. trust tier — the principle this instantiates), card 060 (org boundary as the distribution axis), card 043 (OAuth as the enforcement point), card 059 (what each distribution shape costs in context), and card 034 (Salesforce productizing the DIY MCP pattern — the adjacent motion from the vendor side).
