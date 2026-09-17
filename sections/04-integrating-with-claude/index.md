---
title: "4. Integrating with Claude (MCP and APIs)"
order: 4
status: drafting
---

## Scope

Exposing your own systems and content as tools Claude can call.

**Boundary with section 3.** The card set for integration and the card set for governance overlap almost completely (039–046 answer to both). They are split here on a single question:

- **Section 4 — how do I build it and make it reachable?** Mechanics, the local/remote boundary, hosting.
- **Section 3 — whose permissions does it act with?** Identity, credentials, blast radius.

Cards 039–046 are owned by section 3. Section 4 points at them rather than re-teaching them.

---

## Topic 4.1 — What MCP is, and where it can be reached from

*One hour. Lecture ~20 min, lab ~30 min, takeaway ~10 min.*

### The spine

MCP is **active retrieval** — and the single fact that most often surprises people is that *local MCP servers are invisible to every hosted surface.*

### Lecture arc

**1. Put MCP on the axis from §2.1 (card 011, `ran-it`).**
Don't introduce MCP as a protocol. Introduce it as the active-retrieval end of a spectrum the audience already saw: knowledge the model must choose to fetch, perfectly fresh, skippable. Everything about how it fails follows from that.

**2. Two distribution paths, and the low-friction one is usually right first (card 012).**
Path A: download 5–10 curated documents into a Project. Zero friction, focused, goes stale. Path B: connect the whole live corpus over MCP. Always current, reaches everywhere — and costs a real install-and-authenticate step. **Lead with A, offer B as the upgrade.** The instinct to demo the impressive one first loses people who then never adopt either.

**3. The reachability rule (cards 038, 040 — `docs` + `ran-it`).**
This is the beat worth the hour. **Ask whose computer runs the request.** Yours — Claude Code on your laptop — and a local `.mcp.json` server works. Anthropic's — a published artifact, a hosted Claude surface, Claude Tag — and it *must* be a remote server. There's no configuration that bridges this; local MCP is a development artifact, not a deployment.

**4. Which means hosting is the forgotten second job (card 038).**
A working local server feels finished and is roughly half done. Hosting, authentication, and uptime are the other half, and they're the half that decides whether anyone else ever uses it.

### Lab (~30 min) — build a local server, then try to reach it from a hosted surface

1. Write a small MCP server with two or three tools. Run it locally via `.mcp.json`. Call it from Claude Code — it works.
2. **Now publish an artifact that tries to call the same server.** It can't see it.
3. Deploy the server somewhere public and point the artifact at that instead.

Step 2 is the lab. Being told about the local/remote boundary is forgettable; watching a working tool become invisible is not.

### Takeaway

"It works on my machine" is the default state of an MCP server, and hosted surfaces cannot reach your machine. Decide at the start whether you're building a personal tool or an integration, because that answer changes the work.

---

## Topic 4.2 — Auth for real clients

*One hour. Lecture ~20 min, lab ~30 min, takeaway ~10 min.*

### The spine

An MCP server with static secrets is a **bag of credentials**. An MCP server behind OAuth is a **policy enforcement point**. The upgrade changes what the server is, not just how it authenticates.

### Lecture arc

**1. The starting position is fine, and knowing why it's fine matters (card 041, `ran-it`).**
Under about ten people, `.env.example` plus a password manager is genuinely production-grade. Individuals swap in their own personal access tokens where attribution matters. **The ceiling is support burden, not security** — you migrate when you're doing environment tech support instead of the work, not because someone told you shared secrets are bad.

**2. What breaks at scale is attribution, not encryption (card 042, `inferred`).**
Fifty tokens across N systems stops being workable, so teams centralise into a service identity — and attribution silently relocates from the credential to your application logs. If nobody noticed it moved, nobody built the logging, and it's simply gone.

**3. OAuth relocates the question entirely (card 043, `inferred`; the OAuth half is `ran-it` on davestanton.com/mcp).**
With SSO in front, the server can mint or exchange tokens scoped to *this caller's* entitlements at request time. Authorization becomes the downstream system's own rules rather than something you reimplemented in logs. Revocation becomes real: kill the session, every downstream access dies.

**Be honest about what's unbuilt.** Card 043 carries `[confirm-this: the token-exchange half — minting per-user downstream tokens — has not been built or tested here.]` The OAuth front door exists and runs; the exchange behind it is architecture, not experience. Teach it as the design you'd reach for, not as a thing that's been proven here.

**4. None of this is AI security (card 044, `inferred`).**
Least privilege, token scoping, revocation, identity-aware proxies — ordinary distributed-systems discipline applied to a new surface. For anyone with platform background this hour should feel like **recognition, not instruction**, and presenting it as novel is how you lose the people best equipped to do it well.

### Lab (~30 min) — make the server observe itself

From cards 039 and 041, both `ran-it`.

1. Give an MCP server a **read-only** credential to some system.
2. Add middleware that logs every tool call before forwarding: timestamp, caller, tool, arguments.
3. Run an agent against it, then reconstruct what it did **from the logs alone** — and compare that to the model's own summary of what it did.

The gap between those two accounts is the lesson, and it generalises well past MCP.

### Takeaway

Decide whether your server holds secrets or enforces policy. Everything else — rotation, revocation, attribution — follows from that one choice.

---

## What's verified and what isn't

| Claim | Card | Status | Note |
|---|---|---|---|
| Passive vs. active retrieval axis | 011 | `ran-it` | Daily use |
| Hosted surfaces reach remote MCP only | 038, 040 | `docs` + `ran-it` | Architectural; should be stable |
| `.env` + password manager to ~10 people | 041 | `ran-it` | Ceiling is support burden |
| OAuth front door on an MCP server | 043 | `ran-it` | davestanton.com/mcp runs it |
| Token exchange / per-user downstream tokens | 043 | `inferred` | **Not built.** Do not present as experience |
| Attribution moves to app logs on centralising | 042 | `inferred` | Real at ~50 tokens × N systems; oversold below that |
| Curated Project upload steers a chat well | 012 | `docs` | `[confirm-this: how well does a 5–10 card upload actually steer a chat?]` |
| Custom/enterprise connectors and credentials | 045, 046 | open | Whether they mirror permissions or can hold their own credential is **not established** |
| Skills disclose progressively; MCP loads every schema at connect | 059 | `docs` | Token figures need a primary source; the "37 tools" comparison is **arithmetic, not measurement** |
| Distribution axis is the Claude org boundary, not plan tier | 060 | `n/a` stance; `docs` on Team marketplaces | Filed as a correction to a wrong first framing |
| A skill can't supply a capability the host lacks | 071 | `ran-it` on `allowed-tools`; `inferred` on degradation | Same surface-dependence as 038, one layer down |
| Marketplace publish → install round trip | 070 | `ran-it` | Claude Code verified; claude.ai/desktop registration **not** |

---

## Gaps — where more hands-on time is needed

**0. Partially closed — packaging and distribution.** Cards 069–071 and the built plugin now cover authoring, marketplace publishing and install for Claude Code, and card 059 covers what a connected surface costs in context. What remains open is the claude.ai/desktop half and hosting, below.

**1. The deployment story is the wall, and it's missing.** This is the highest-value gap in the section. The cards say hosting is the forgotten second job and then don't cover it: how to host an MCP server so a team can reach it safely, what the auth story is once it's public, how you know it's up. *"Build locally, host it, keep it up"* separates a proof of concept from a system, and right now the corpus stops at the proof of concept. **Needed: deploy one properly and card the whole path.**

**2. Token exchange is designed but unbuilt.** Card 043's second half is the load-carrying claim of topic 4.2 and it's `inferred`. Building it once against one real downstream system would convert the strongest architectural argument in the section from reasoning into experience.

**3. No IdP integration guidance.** OAuth is treated as a given. How an MCP server actually plugs into an existing Okta/Entra/Auth0 setup — OAuth client? SAML forwarding? — is absent, and it's the first question any enterprise asks.

**4. No testing story for MCP servers.** Card 022's feedback-loop discipline is never applied to MCP itself. How do you test a server? How do you verify a tool *description* is right before shipping it, given the description is what the model actually reads?

**5. Secrets rotation without downtime.** The principles are carded; the mechanics of rotating one credential across N running agents without dropping traffic are not.

**6. No incident story.** What the operational signal is, who notices when a server breaks, how you roll back a bad MCP deploy without rolling back Claude.
