# Two Access Models Side by Side — Connectors *Mirror Your* Permissions, Agent Identity Gives Claude *Its Own* Accounts

**Source:** Surfaced while verifying a Chat-vs-Cowork question, Claude Boot Camp session 2026-09-11. Verified against [Use Google Workspace connectors](https://support.claude.com/en/articles/10166901-use-google-workspace-connectors), [Use connectors to extend Claude's capabilities](https://support.claude.com/en/articles/11176164-use-connectors-to-extend-claude-s-capabilities), and [Agent identity: a new access model for autonomous, team-wide AI](https://claude.com/blog/agent-identity-access-model).
**Type:** product-behavior
**Verified:** `docs` — direct quotes below. **Not run.** `[confirm-this: whether a custom/enterprise MCP connector follows the mirroring model or can be given its own credential — not established.]`
**Relevant to:** 3 (administering — governance), 4 (integrating), 2 (operating Claude products), 1 (foundational concepts)

## The two models

Card 039 asked the abstract question — *whose permissions does this agent act with?* Anthropic's products answer it **both ways**, on purpose, for different situations.

### 1. Connectors — Claude acts as you

> *"Claude **mirrors your existing permissions** — you cannot access information you don't already have access to in Google Workspace."*
>
> *"Even when you allow a write action in Claude, a person still needs the underlying permission in the source system to make that change, and restricting actions in Claude never grants more access than the source system permits — **it only narrows it**."*

This is the ambient-permission model from card 039, and it validates the original concern: your access *is* the ceiling. Admin controls can narrow what Claude will attempt; they can't change **whose** permissions it attempts it with.

The design is sound for single-player use — you can't use it to reach anything you couldn't already reach. It's exactly the wrong shape when the human happens to be an admin on production.

### 2. Agent identity — Claude acts as itself

Claude Tag inverts it:

> *"Claude isn't acting on behalf of a single user. **It has its own account in each system it touches**: it posts in Slack as the Claude app, opens pull requests as the Claude GitHub App, and queries your warehouse under a service account."*

Admins provision separate service accounts per system, and **different keys at different permission levels per channel** — read-only warehouse access in general channels, write access in specialised ones. Workspace-level baseline profiles, overridden per channel. Private channels get distinct identities; public channels share the workspace-level one.

The stated reason is multiplayer: in a shared channel, acting as any one user would make the channel *"a side door into someone's private documents."*

## Side by side

| | **Connector model** | **Agent identity** |
|---|---|---|
| Acts as | **You** | **Itself** — per-system service accounts |
| Ceiling | Your own access | Whatever the admin provisioned |
| Admin controls can | Only narrow | Set the scope outright |
| Right for | Single-player, my-own-data work | Shared channels, autonomous team work |
| Fails when | The user is over-privileged | Nobody scopes the service accounts carefully |
| Where | Chat, Cowork connectors | Claude Tag |

## Attribution — the part card 042 predicted

Card 042 warned that moving to a service identity relocates *"which human did this"* out of the credential and into application logs you have to build.

Anthropic built it:

> *"Every routine, memory write, and network call made with agent credentials is recorded, and **because Claude acts under its own service accounts, those actions also land in each connected system's own logs**."*

So you get both halves: Anthropic's record of who asked, and the target system's record of the service acting. That's the mapping card 042 said had to live somewhere — here it's a product feature rather than your homework.

## Why this matters

It turns "is this safe?" into a checkable question with two possible answers, and the answer is a property of the *surface*, not the vendor. Same company, same model, opposite identity models — chosen to fit the use case.

The practical rule: **ask which model a surface uses before deciding whether a privileged person should use it.** Connector-model surfaces inherit whatever the human has, which makes them riskiest in exactly the hands that look most qualified — admins and platform engineers.
