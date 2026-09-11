# OAuth Turns an MCP Server From a Bag of Secrets Into a Policy Enforcement Point

**Source:** Dave's move toward OAuth/SSO in front of MCP servers at scale, Claude Boot Camp session 2026-09-11. Architectural consequences are Beta's, same session.
**Type:** concept
**Verified:** `inferred` for this architecture as applied to a team's internal MCP fleet — reasoned, not built. `ran-it` for the narrower fact that **an MCP server can sit behind OAuth**: davestanton.com/mcp implements OAuth 2.1 + Dynamic Client Registration and is in daily use. `[confirm-this: the token-exchange half — minting per-user downstream tokens — has not been built or tested here.]`
**Relevant to:** 4 (integrating — MCP and APIs), 3 (administering — governance), 5 (building with Claude Code)

## The shift

With per-user PATs or a shared service account, an MCP server is a **dumb pass-through holding a static bag of secrets**. It has whatever access it was configured with, and every caller gets all of it.

Put OAuth/SSO in front of it and the server becomes a **policy enforcement point**: it can mint or exchange tokens scoped to *this specific caller's* entitlements, at request time.

That's not a credential swap. It changes what the component is.

## Why this beats attribution logging

Card 042 framed the scaling problem as a choice between per-user tokens (expensive) and a service identity (loses attribution, which you then rebuild in application logs). **OAuth token exchange is a third path, and a strictly better one:**

| | Shared PAT / service account | **OAuth token exchange** |
|---|---|---|
| "This user did X" | Best-effort **attribution**, logged alongside a credential that could have done more | The granted permission **is** the user's permission |
| If the user lacks access to a Sumo index or AWS account | The credential can still reach it; only your logging says who asked | **The token literally can't reach it** |
| What you're relying on | Your own discipline about logging | The downstream system's own authorization |

> **That's authorization, not an audit trail.** The difference is whether the guardrail is enforced by the system that owns the data, or by a log you wrote.

It also fixes the revocation problem PATs never solve at scale: **kill an SSO session or a group membership and every downstream access dies with it** — no rotation coordination across N systems, no chasing a token someone copied into a `.env` six months ago.

## What the server now has to do

This is real infrastructure, not `read .env at startup`:

- **Validate** the incoming OAuth/OIDC token from your identity provider
- **Map** claims and group memberships to downstream scopes
- **Forward or exchange** a token per downstream system (an on-behalf-of / token-exchange flow)
- Handle **session and token caching**, **refresh**, and **per-downstream client registration**

Each downstream system is its own integration. That cost is the honest counterweight to everything above.

## Gate the server itself, not just what it calls

Put the **MCP server behind the identity provider too** — not only the calls it makes downstream.

That's what preserves card 040's property (*nothing exposed has real access*) once compute moves off your laptop to shared infrastructure. The tunnel worked because access was physically impossible; at scale, access is gated the same way as everything else in the estate. Different mechanism, same guarantee.

## Build or buy

For a Python stack, this is the point where an **OIDC middleware layer** or a **managed identity-aware proxy** in front of the MCP servers earns its keep, rather than hand-rolling exchange logic in every server. Token validation, refresh, and per-system client registration are exactly the code you don't want written five times with five sets of bugs.

**Open question:** build the token-exchange layer **once, as shared infrastructure** that other teams' MCP servers sit behind — or per-tool? Shared is the obvious answer on paper and makes the layer a dependency everyone's tooling now waits on. `[confirm-this]`

## Why this matters

It's the answer to "how does this work when it's not just my laptop" that doesn't give up the security property you started with. The naive scaling path — one service account with broad access, plus logging — is how internal tools quietly accumulate more privilege than any of their users have.

For a CoE lead this is also the architectural bar to hold vendors and internal teams to: *does the integration act with the caller's entitlements, or with its own?*
