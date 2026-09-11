# Per-User Tokens vs. Service Identity — Centralizing Moves Attribution From the Credential Into Your Logs

**Source:** Beta, Claude Boot Camp session 2026-09-11, on what gets harder rather than easier when a small-team secrets setup scales.
**Type:** concept
**Verified:** `inferred` — reasoned from the mechanics, not measured. The small-team half is `ran-it` (card 041). `[confirm-this: no worked example yet of the application-level attribution logging this implies.]`
**Relevant to:** 3 (administering — governance), 4 (integrating), 1 (foundational concepts)

## The thing that gets harder, not easier

Scaling a secrets setup is usually framed as strictly an improvement. One property quietly gets **worse**: knowing which human did what.

At four people, per-user personal access tokens are trivial — everyone manages their own, and the target system's log says *"Dave did this."* Attribution is free, and it lives **in the credential**.

At a hundred people you pick one of two costs:

| | **Keep per-user tokens** | **Move to a service identity** |
|---|---|---|
| Attribution | Still free, still in the credential | **Lost at the credential layer** |
| What you now operate | Onboarding, offboarding, and rotation for 100 tokens × N systems | One credential |
| The audit question becomes | *"Which person did this?"* — answered by the target system | *"Which service did this, on behalf of which person?"* |
| Where that mapping lives | Nowhere — you don't need one | **Your application logging.** It has to be built |

Neither is wrong. But the second one **relocates a property you were getting for free** and, if nobody notices, simply deletes it.

> **The binary above is false, and card 043 is the third path.** With OAuth token exchange, the MCP server mints per-user downstream tokens at request time — so you get the user's *actual* entitlements without operating 100 PATs, and attribution stops being a logging problem because the permission itself is theirs. Read this card as *what happens if you don't do that*; it's the common outcome, not the good one.

## The failure mode

You centralize secrets for good reasons (card 041), switch to a service account because managing 100 tokens is absurd, and six months later someone asks who triggered a change. The target system says the service did. Your logs say nothing, because nobody scoped "preserve attribution" as a requirement of the migration — it wasn't a feature anyone chose, so it wasn't a feature anyone protected.

This is the same shape as card 039's blast-radius question, one layer up. There it was *whose permissions bound the action*; here it's *whose identity records it*. Both are properties of the credential model, and both change silently when the credential model changes.

## The rule

> **When you move from per-user credentials to a service identity, attribution becomes an application-level responsibility. Budget for it in the migration, not after.**

Concretely: the agent, harness, or tool must log the acting human alongside every call it makes on their behalf — and that log has to be as durable and searchable as the target system's own audit trail, since it's now the only place the mapping exists.

Card 039's advice applies directly: log what calls *did*, from the tooling, independent of the model's own account of itself. At small scale that's a good habit. Once you're on a service identity, **it's the audit trail.**

## Why this matters

It's a requirement that's easy to discover late and expensive to retrofit, because the data you needed wasn't being written. Nobody decides to lose attribution — it's a side effect of a decision that looked purely operational.

For a center-of-excellence lead it's also the right question to ask about any vendor's "enterprise" integration: *when this acts on a user's behalf, where is that recorded, and for how long?* Plenty of products centralize access beautifully and answer that question with a shrug.
