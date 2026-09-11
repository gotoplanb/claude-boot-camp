# Claude Tag Replaced "Claude in Slack" — Org Identity, Not Per-User, With Admin-Provisioned Access Bundles

**Source:** Raised by Dave as a product to explore, Claude Boot Camp session 2026-09-11. Verified against [Claude Tag overview](https://claude.com/docs/claude-tag/overview), [Give Claude access to your tools](https://claude.com/docs/claude-tag/admins/add-connections), and [What is Claude Tag?](https://support.claude.com/en/articles/15594475-what-is-claude-tag).
**Type:** product-behavior
**Verified:** `docs` — read the official pages. **Not yet run.** `[confirm-this: needs a real Team/Enterprise setup — see the open questions.]`
**Relevant to:** 2 (operating Claude products), 3 (administering), 4 (integrating — MCP)

## What changed

**Claude in Slack switched to Claude Tag on 3 August 2026.** It isn't a rename — the access model inverted.

| | Old "Claude in Slack" | **Claude Tag** |
|---|---|---|
| Identity | Per user | **The organization's** shared identity |
| Setup | Each person connects their own | **An Owner provisions once**, scoped to channels |
| Plans | Individual plans included | **Team and Enterprise only** — not Free/Pro/Max, not third-party deployments |
| Where work runs | — | **Ephemeral Anthropic-hosted sandbox**, created per conversation, discarded when idle. Not your machine, not your network |

You tag `@Claude` in a channel, thread, or DM and hand it work; it posts a visible checklist in the thread, and the whole exchange stays visible to the channel.

## Access bundles

An Access bundle is *"a named set of credentials, repository grants, and instructions that Claude uses in the channels the bundle covers."* Owners create them at `claude.ai/admin-settings/claude-tag` and scope them **per channel, workspace, or organization**.

- **A connection** is one service credential inside a bundle — a Datadog API key, a warehouse service account — usable from any channel under that bundle's scope.
- **Plugins** (bundles of skills) attach to the same bundle, *"so the credential arrives with directions for using it."* That pairing is a nice bit of design: access and instructions travel together rather than the model having a key and no idea when to use it.
- The preset Connect buttons **aren't the full set** — *"Any app with an API can be connected"* via **Custom tool**, including a custom MCP server (card 038).

## The billing split, which is easy to miss

- **Channel and thread work** draws from *"a usage balance, an amount in your organization's billing currency that an Owner funds."*
- **Direct messages use the sender's personal claude.ai account** and don't touch the org balance.

So the same product bills two different ways depending on where you type. Worth knowing before someone concludes DMs are free to the org — they're free to the *org*, and metered against the individual's plan. That's card 035's cost-shape question landing inside one product.

## Open questions

- **Can a claude.ai Project's knowledge base be attached to a bundle?** The docs describe bundles in terms of credentials, repos, and instructions — Projects aren't named. Likely you'd expose those files through a connector (Drive, your own MCP) instead. `[confirm-this]`
- The docs' own suggested diagnostic once it's set up: ask **`@Claude what can you access from this channel?`**

## Why this matters

It's a genuinely different governance story from per-user AI tools, and the direction is toward *more* control, not less: admin-only provisioning, per-channel scoping, an org-funded balance, and work in a sandbox that never touches company machines. For a CoE lead, that's most of the control surface you'd want to ask for.

It also puts a real answer behind "can we just ask our data questions in Slack?" — with the caveat that the answer runs through whatever bundle an Owner scoped to that channel, which is the actual security boundary to reason about.
