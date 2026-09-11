# The Remote-Control Tunnel — Keep Compute and Credentials Private, Expose Only the Control Channel

**Source:** Dave's working setup, Claude Boot Camp session 2026-09-11. The public/private subnet analogy is Beta's, same session.
**Type:** preference
**Verified:** `ran-it` — this is how Dave drives Claude Code from his phone today (ngrok, local harness, Python).
**Relevant to:** 5 (building with Claude Code), 4 (integrating), 3 (administering — governance)

## The shape

Drive a local Claude Code session from your phone without putting anything real on the internet:

- **Private perimeter — your laptop.** The agent, the credentials, the MCP servers, the actual access. None of it reachable from outside.
- **Exposed surface — a thin, ephemeral control channel.** An ngrok tunnel carrying the chat interface, nothing else.

> **The tunnel carries UI, not keys.**

That's the whole security argument. Nothing on the public internet needs a hardening review because **nothing on the public internet has real access** — compromise the tunnel and you get a chat window, not a credential.

It's public/private subnet thinking applied to agent tooling: the workload sits in the private subnet, and only a narrow control path is exposed. The pattern is old; what's new is applying it to where an agent's *compute* lives rather than where a server does.

## Why this beats the obvious alternative

The obvious way to get phone access is to use a hosted product that already runs in the cloud. That trades the tunnel's setup cost for a different access model — and if the hosted option authenticates **as you**, you've swapped a bounded blast radius for an unbounded one (card 039).

The tunnel keeps the good property: **scoped local credentials**, with the phone as a remote control rather than a second point of access.

## It also sidesteps the remote-MCP constraint

Card 038 says hosted surfaces reach only *remote* MCP servers — a local `.mcp.json` is invisible to Anthropic's infrastructure.

**The tunnel pattern doesn't hit that**, and it's worth seeing why: the request is executed by Claude Code *on your laptop*, so local MCP servers work normally. You only inherit the remote-only requirement when the compute moves to someone else's infrastructure (Claude Tag, published artifacts).

So the two cards aren't in tension — they're the same rule seen from both sides. **Whoever runs the request determines what it can reach**, and this pattern deliberately keeps that "whoever" as your own machine.

## The "worth the squeeze" test

Before building anything hosted and publicly reachable, ask whether a **local, read-only MCP against one system** would validate the idea.

Usually it would. Hosting, auth, and uptime (card 038's forgotten second job) are real work, and most experiments die before they'd justify it. Reaching for the tunnel first means the cost of a failed experiment is an afternoon rather than an afternoon plus an exposed service you now have to maintain or remember to tear down.

## Why this matters

It's a concrete answer to "I want AI help on my phone without giving a cloud product my production access" — a question every practitioner eventually asks, usually framed as a product choice when it's really an architecture choice.

Stated generally: **exposure and capability are separate dials.** The instinct is to assume remote access requires remote capability. It doesn't — you can expose a control channel with almost no capability at all and keep every dangerous thing behind it.
