# Claude's Google and Microsoft Connectors Hold One Account Each — a Second Sign-In Replaces, Rather Than Adds

**Source:** Dave, Claude Boot Camp session 2026-09-17, working out whether to connect several Google accounts at once.
**Type:** product-behavior
**Verified:** mixed — see the table. This is the account-multiplicity companion to card 046, which established the *permission* model from the docs but not how many identities a connector can hold.
**Relevant to:** 2 (operating Claude products), 4 (integrating), 3 (administering — governance)

## What's verified and what isn't

| Claim | Status |
|---|---|
| A second Google sign-in **replaces** the connected account rather than adding alongside it | `ran-it` |
| Gmail, Calendar and Drive share one Google sign-in, so the swap moves all three together | `ran-it` |
| Swap takes about a minute — Customize → Connectors → disconnect → reconnect | `ran-it` |
| Swapping affects only what Claude can see; chats, memory and projects are untouched | `ran-it` |
| Individual services can be toggled off, leaving e.g. Drive on without Gmail/Calendar | `ran-it` |
| One Google **and** one Microsoft 365 account can be connected at the same time | `inferred` — separate products with separate auth. *[confirm-this: not tested; no M365 tenant to hand.]* |
| A third-party/custom MCP bridge holding multiple OAuth logins can exceed this | `inferred` — mechanism is sound, none built |

## The constraint

The built-in Google Workspace connector and the Microsoft 365 connector each authenticate against **one account at a time**.

Signing in with a second Google account doesn't give you two — it swaps which one Claude can see. And because Gmail, Calendar and Drive all ride the same Google sign-in, they move together; there's no holding personal Gmail and work Drive simultaneously.

Cross-provider is the exception: Google and Microsoft are separate products with separate auth, so one of each can coexist. **Two accounts on the same provider** — two Gmails, two M365 tenants — is where the native connectors stop.

## Why the swap is cheaper than it sounds

Disconnect and reconnect takes roughly a minute, and it's **non-destructive to everything except visibility**. Chats, memory and projects are unaffected; the only thing that changes is which inbox, calendar and drive Claude can reach.

That matters for the decision in card 066: if you genuinely need two accounts only occasionally, swapping is a real answer rather than a workaround you're settling for. The constraint bites when you need *simultaneous* access, not alternating access.

**Scope it down too.** The services are individually toggleable, so a connector attached for one purpose — Drive for a client account, say — can have Gmail and Calendar switched off. That's card 039's minimum-scope instinct applied to a consumer surface: connect the account, but not every service it carries.

## The escape hatch, and what it costs

A third-party bridge or custom MCP server holding multiple OAuth logins can present several same-provider accounts at once. The mechanism is sound.

The cost is that **your mail and documents then route through another party's infrastructure**. That's a real trade-off, not a free upgrade — and it converts a first-party connector whose permission model is documented (card 046) into a dependency whose model you have to establish yourself.

## Why this matters

It's the kind of limit that's invisible until it bites, because the UI presents signing in again as an ordinary action rather than a replacement. Knowing it up front changes how you set up the connection — one account chosen deliberately, services trimmed — instead of discovering it when the wrong inbox is attached.

See card 046 (whose permissions a connector acts with — this card covers *whose account*), card 039 (minimum scope), card 066 (whether to connect at all), and card 060 (before building a bridge: is this reach you need, or friction you're routing around?).
