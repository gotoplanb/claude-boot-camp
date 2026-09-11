# Ambient User Permissions vs. Scoped Service Tokens — What Actually Bounds the Blast Radius

**Source:** Dave, Claude Boot Camp session 2026-09-11, articulating why his governance hesitation about Claude Cowork is specific rather than general. Token-hygiene and audit points are Beta's, same session.
**Type:** concept
**Verified:** `ran-it` for the local scoped-token setup — this is how Dave runs Claude Code today. The **security principle** is reasoned and uncontroversial. `[confirm-this: **Cowork's access model remains unverified after two documentation searches** — see card 045 and issue #7. Do not cite this card as a critique of that specific product until it's settled; the principle stands on its own regardless.]`
**Relevant to:** 3 (administering — governance), 4 (integrating), 2 (operating Claude products)

## The question that decides the surface

Not *"is this tool safe?"* but:

> **When this agent acts, whose permissions is it acting with?**

Two answers, and they bound risk completely differently:

| | **Scoped service token** | **Your ambient permissions** |
|---|---|---|
| The agent can do | Exactly what the token allows | **Exactly what you can do** |
| Blast radius of a mistake | The token's scope | Your whole access footprint |
| Bounded by | A thing you configured | A thing you accumulated over years |

Claude Code running locally, fed a read-only credential through env vars or a secrets manager, is bounded by that credential. A mistake, a prompt injection, or a hallucinated tool call can't exceed it.

A tool that authenticates **as you** inherits everything you happen to have. And here's the part that matters:

> **If you're an admin on production systems, "bounded by what you can do" is not a bound at all.**

That's the whole hesitation, stated precisely. It isn't anxiety about AI at work — it's a specific objection to an access model, and it applies to any tool with that model regardless of vendor. Stating it this way also makes it *answerable*: a product that adopts scoped credentials resolves the objection, and one that doesn't, doesn't.

## Credential hygiene that follows

- **Read-only wherever the task allows.** Most exploratory work never needs write access, and read-only is the cheapest possible blast-radius reduction.
- **Short-lived where the system supports it.** A leaked token that expires is an incident; one that doesn't is a liability.
- **Scope per MCP server, not one shared credential.** One credential reused across servers means a single compromised server **fans out** to everything that credential reaches. Separate tokens keep a compromise contained to one blast radius.

## Log what the calls *did*, separately from what Claude said

Keep the local harness logging each MCP call's actual effect — **independent of Claude's own reasoning trace.**

The reasoning trace is the model's account of itself. That's useful, and it is not ground truth. When you need to reconstruct *what touched what*, you want a record produced by the tooling rather than narrated by the thing under investigation. Same principle as card 022: observe, don't accept the report.

This matters more, not less, when you hold admin rights elsewhere — the person with the broadest access is the one who most needs to be able to prove what happened.

## How the secrets are actually managed

**Answered — see card 041.** `.env.example` committed per repo, the real `.env` in a password manager scoped to the project's people, and individual personal access tokens swapped in for systems where the log should name a human. Card 042 covers what happens to that last property when the approach outgrows a small team.

## Why this matters

It converts a vague reservation into a **procurement-grade question** you can ask any vendor: *does this act with delegated user identity, or with a credential I scope?* The answer is checkable, and it predicts the risk profile better than any amount of trust-and-safety documentation.

For a CoE lead, it's also the right shape for policy — it's a property of the integration, not a judgment about AI, so it survives model and product churn.
