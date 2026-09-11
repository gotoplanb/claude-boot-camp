# Hosted Surfaces Reach *Remote* MCP Servers Only — Your Local `.mcp.json` Is Invisible to Them

**Source:** Noticed across two separate verifications in the Claude Boot Camp session of 2026-09-11 — the same constraint appeared for published artifacts and for Claude Tag. Verified against [Claude Tag — add connections](https://claude.com/docs/claude-tag/admins/add-connections) and [Share session output as artifacts](https://code.claude.com/docs/en/artifacts).
**Type:** gotcha
**Verified:** `docs` for both surfaces. `ran-it` for the underlying distinction — Dave's own MCP server is remote and public (`davestanton.com/mcp`), which is why it works from the phone at all.
**Relevant to:** 4 (integrating — MCP and APIs), 2 (operating Claude products), 5 (building with Claude Code)

## The rule

> **If Anthropic's infrastructure has to make the call, the MCP server must be reachable from the internet. A server configured in your local `.mcp.json` is invisible to it.**

Two independent confirmations:

- **Claude Tag:** *"when you add a custom connector, Claude connects to your remote MCP server from Anthropic's cloud infrastructure, rather than from your local device."*
- **Published artifacts:** local MCP servers *"can supply data while Claude builds the page, but the published page can't call them"* (card 029).

The second is the sharper illustration, because it shows the line running *through a single workflow*: your local server can feed the build, because Claude Code is running on your machine. The published page runs on Anthropic's infrastructure and cannot reach it.

## Where the line falls

| Surface | Local `.mcp.json` | Remote MCP server |
|---|---|---|
| Claude Code session on your machine | ✅ | ✅ |
| Building an artifact from that session | ✅ | ✅ |
| **Published artifact, viewed by someone** | ❌ | ✅ |
| **Claude Tag in a Slack channel** | ❌ | ✅ |
| Claude apps / Projects connectors | ❌ | ✅ |

The test is simple: **whose computer runs the request?** Yours → local works. Anthropic's → it must be remote.

## What this means in practice

**A local MCP server is a development artifact, not a deployment.** It's great for building and iterating. The moment the plan involves anyone else — a shared Slack channel, a published dashboard, a teammate opening a link — it has to be hosted somewhere with a public URL and real auth.

That reframes "build an MCP server" as two jobs, and the second is the one people forget:

1. Make the tools work (the fun part, done locally in an afternoon).
2. **Host it, authenticate it, and keep it up** — the part that decides whether anyone but you can ever use it.

For anything internal, step 2 is also where the security work lives: an internet-reachable server exposing company data needs real authentication, not a token pasted in a config. That's the whole private-MCP design question already parked in [issue #2](https://github.com/gotoplanb/claude-boot-camp/issues/2).

## Why this matters

It's a constraint that only shows up at the moment of sharing — after the demo works, after you've told someone it works. The local version proves the *tools* are right and proves nothing about whether the thing is deployable.

For a consultant scoping work, it's the difference between "I'll build you an MCP integration" as an afternoon and as a project with hosting, auth, and an owner.
