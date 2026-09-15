# Skills Load on Demand, MCP Tool Schemas Load Every Session — the Context Bill Arrives Before Your First Message

**Source:** Dave, Claude Boot Camp session 2026-09-14, working through whether to package something as a Skill or as a custom MCP server. Figures are from Dave's reading during that session; the primary source was not captured — see the table.
**Type:** concept
**Verified:** mixed, deliberately — see the table below.
**Relevant to:** 4 (integrating — MCP and APIs), 2 (operating Claude products), 5 (building with Claude Code)

## What's verified and what isn't

| Claim | Status |
|---|---|
| Skills use progressive disclosure — name + one-line description at session start, full instructions only when judged relevant | `docs` |
| Classic MCP loads every connected tool's full schema into context whether or not it's used | `docs` — and consistent with `ran-it` observation of context filling on connect |
| ~100 tokens per skill listing; up to ~5,000 tokens for a loaded skill body | `docs` — *[confirm-this: link the primary source. Read during the session, URL not captured.]* |
| ~550–1,400 tokens per MCP tool | `docs` — same caveat, same missing link |
| "37 tools ≈ 20k–50k tokens; 37 skills ≈ 3,700" | **arithmetic**, not measurement — the per-unit figures above multiplied out. Treat as an order-of-magnitude illustration. |
| Tool Search retrofits progressive disclosure onto MCP tools in Claude Code | `docs` |
| Whether Tool Search applies the same way to a custom MCP connector from claude.ai chat | `confirm-this` — **do not assume it does** |
| A shared MCP server's tool list refreshes on the client's own polling/TTL cycle | `inferred` from how connectors behave; not measured |

## The mechanism difference

**Skills are lazy.** At session start only the name and a one-line description load — enough for the model to decide relevance, and nothing more. The body loads when it's actually wanted. Discovery is cheap; content is paid for on use.

**Classic MCP tools are eager.** The full schema — name, description, JSON schema, field descriptions, enums — loads for *every connected tool*, used or not. You pay the whole bill at connect time, before the first message.

That is the entire tradeoff, and it is a difference in *when* the cost lands, not in what the thing can do.

The practical consequence is that MCP cost scales with **everything you connected**, while Skills cost scales with **what you actually invoked**. A large tool surface is therefore not neutral: it is a standing tax on every session, including the sessions that never touch it.

**Tool Search is the mitigation** — it collapses MCP tools to a name-only listing and fetches schemas on demand, which is Skills' trick applied to MCP. Confirmed for Claude Code. **Not confirmed for a custom MCP connector used from claude.ai chat**, and that gap matters precisely because the custom-connector case (card 060) is the one where you'd most want it. Test it rather than assuming it carries over.

## The one thing MCP does better here

**Updates propagate without asking anyone to do anything.** A shared MCP server's tool list refreshes on the client's own polling/TTL cycle — change it server-side once and clients catch up on their own.

A distributed skill package has no such path: every person has to re-pull. For a fixed, cooperative team that is a minor chore. For a set of people you have no admin relationship with, it is the difference between an update landing and an update not landing.

So the axes cross: **Skills win on context economy, MCP wins on update propagation.** Neither is the general-purpose answer.

## Why this matters

This is card 011's passive-context/active-retrieval axis with a price attached. Knowing that MCP is active retrieval doesn't tell you it bills you at connect time for retrievals you never make — and that's the part that decides real packaging questions.

It also reframes a common complaint. "Claude feels dumber with all my connectors on" is usually not a model problem; it's the context budget spent before the conversation started. The fix is fewer connected tools, or Tool Search, not a different model.

See card 011 (the axis this prices), card 012 (a curated Project upload as the cheapest distribution of all — no schema cost whatsoever), card 038 (hosted surfaces reach remote MCP servers only), and card 060 (which of the two to build, decided on org boundary rather than cost).
