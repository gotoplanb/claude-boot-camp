# None of This Is AI-Specific — It's Ordinary Architecture Discipline, With a Bigger and Faster Blast Radius

**Source:** Dave, Claude Boot Camp session 2026-09-11, stepping back from the MCP identity thread. The magnitude-not-kind sharpening is Beta's, same session.
**Type:** concept
**Verified:** `inferred` — a judgment, grounded in Dave's SRE and platform-engineering background rather than a measurement.
**Relevant to:** general, 1 (foundational concepts), 3 (administering)

## The observation

Everything in the MCP identity arc — least privilege, token scoping, per-user vs. service identity, identity-aware proxies, centralized revocation, crawl/walk/run rollout — is **ordinary distributed-systems and access-control discipline**. There is nothing new here because an LLM is involved. There's just an LLM sitting at one end of the call now.

The consequence for who gets hurt:

> **People won't get burned by anything novel about MCP.** They'll get burned by ambient credentials, unscoped tokens, and no revocation story — the same things that have always burned people who skipped fundamentals.

Which means the risk concentrates in a specific place: **teams implementing AI tooling without the architectural background to recognise patterns they've already got answers for.** If your AI implementers haven't run production systems, they will meet these problems as novel and solve them badly — or not notice them until something goes wrong.

## What the agent actually changes

Not the *kind* of failure. The **magnitude and speed**.

A script with an over-scoped token does one wrong thing, once, when you run it. An agent with the same token does many things, quickly, in an order nobody specified, while pursuing a goal you stated loosely. Same category of mistake; different size of crater.

That's the honest framing and it's worth holding both halves:

- **Don't mystify it.** "AI security" as a wholly new discipline sells products and obscures the fact that the fixes are the ones you already know.
- **Don't dismiss it either.** Faster and broader failures justify tightening controls you might have left loose for a script, because your margin for noticing has shrunk.

## Why this matters

It tells you who to put on this work. The instinct is to staff AI projects with people excited about AI. The constraint that actually binds is whether someone on the team has operated systems with real credentials — and that skill transfers in completely, while enthusiasm doesn't substitute for it.

For the curriculum's audience (card: who it's for), it also sets the teaching posture: for anyone with a platform background, **most of section 3 should feel like recognition, not instruction.** The job is to map familiar patterns onto a new surface, not to present them as new. Presenting them as new is how you lose credibility with the exact people best equipped to do this well.
