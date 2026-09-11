# Cowork Is for the Non-Engineering Majority — It Solves Access Control *For* You, Not *By* You

**Source:** Dave asked which companies and use cases Cowork actually fits, Claude Boot Camp session 2026-09-11 — steelmanning rather than dismissing. Verified against [Making Claude Cowork ready for enterprise](https://claude.com/blog/cowork-for-enterprise), [role-based permissions](https://support.claude.com/en/articles/13930458-set-up-role-based-permissions-on-enterprise-plans), and the [Cowork Enterprise Admin Guide](https://claude.com/resources/tutorials/claude-cowork-enterprise-administrator-guide).
**Type:** product-behavior
**Verified:** `docs` throughout. The authentication model was unresolved when this card was written and is now answered — see the closing section and card 046. **Not run.**
**Relevant to:** 2 (operating Claude products), 3 (administering), 1 (foundational concepts)

## The data point that answers the question

> **The vast majority of Cowork usage comes from non-engineering teams** — operations, marketing, finance, legal.

Typical work: project updates, collaboration decks, research sprints. Real customer examples in the announcement: engineering-bottleneck dashboards (Zapier), guided performance reviews (Jamf), board-prep workflows pulling from Drive, Slack, and competitor data (Airtree).

That reframes the comparison entirely. **Cowork isn't competing with a hand-built MCP harness** — it's serving a population that will never build one.

## Who it's for

**People who can't and shouldn't have to build an MCP harness.** The local-Claude-Code-plus-scoped-tokens pattern (cards 040, 041) requires someone who thinks in env vars, secrets managers, and API scopes. Most of a company doesn't and won't. "No coding required" is the whole pitch, not a marketing softener.

**Broad, shallow, document-and-workflow work** — reading a stack of files to produce an FAQ, triaging documents, aggregating a survey into a trend report. Not "admin-adjacent access to a production observability system."

**Organisations that need the permission model solved *for* them.** This is the part that directly answers the governance objection, because it's the fine-grained scoping layer you'd otherwise hand-roll, shipped as a product:

- **Role-based access control** — custom roles carrying the Cowork entitlement, assigned to groups
- **Per-tool connector permissions** — restrict which actions are available *within* each MCP connector org-wide; **allow read, disable write**
- **Group spend limits**
- **OpenTelemetry** events for tool/connector calls, file operations, skill usage, and approval status (manual vs. automatic) — compatible with **Splunk and Cribl**, with a **shared user account identifier** for correlating against Compliance API records

A quirk worth knowing: **roles are additive** (a user in several groups gets the union of permissions) while **spend limits take the most restrictive** across groups, with the org cap as a hard ceiling. Opposite rules for the two things, which is exactly the kind of asymmetry that produces a surprise in an audit.

Anthropic built that layer because an org deploying to 500 non-technical employees has no ability to hand-roll it. **That's the actual product.**

## Where the hand-built pattern still wins

Even inside a company that has adopted Cowork:

> **Anything where the access itself is the sensitive part.**

Production systems, admin-adjacent credentials, cases where "read-only Sumo Logic token, four known people" is the correct blast radius (card 039). Cowork's connector controls govern *which actions* a connector can take, but the shape remains **broad tool, many systems, many people**. That's a different risk profile from **narrow tool, one system, four people who each know exactly what it does**.

SRE, platform, and infra tooling built for your own team is close to the canonical case *for* the hand-built pattern — not an argument against Cowork.

## The honest split

| | **Cowork** | **Purpose-built MCP** |
|---|---|---|
| Scales | General AI-assisted work across a non-technical workforce | One tool, one job, deeply |
| Access control | Solved for you, centrally | Chosen by the person who understands the blast radius |
| Failure if misapplied | Deep access granted broadly | Every team reinventing access control badly |

## How it authenticates — the crux, now answered

The question this card originally left open: does Cowork act as the signed-in user with their ambient permissions, or with separately-configured connector credentials?

**As the user — see card 046.** The connectors docs state that Claude *"mirrors your existing permissions"* and that restricting actions in Claude *"never grants more access than the source system permits — it only narrows it."* Since Cowork uses the same connectors, the connector path is **user-permission mirroring**, not managed service credentials. The per-tool admin controls narrow *what* it will do, not *whose* access it does it with.

Still open: whether a **custom or enterprise MCP connector** can be given its own credential, and how Cowork's **local file access** is scoped. `[confirm-this — issue #7]`

## Why this matters

It replaces a posture with a fit assessment. "I wouldn't use Cowork" and "Cowork is wrong" are different claims, and only the first was ever warranted: the objection was about a specific access shape for a specific kind of work, and most Cowork usage isn't that kind of work.

For a CoE lead, both tools are usually correct simultaneously — Cowork for the workforce, purpose-built MCP for the teams whose tooling touches production.
