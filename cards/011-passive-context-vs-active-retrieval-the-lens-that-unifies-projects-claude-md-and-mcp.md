# Passive Context vs. Active Retrieval — The Lens That Unifies Projects, CLAUDE.md, and MCP

**Source:** Beta seminar session 2026-09-11, generalizing the Projects-vs-CLAUDE.md comparison in card 008 into a single axis.
**Type:** concept
**Verified:** `ran-it` for both poles — CLAUDE.md and MCP are in daily use (the site's MCP server answered a live `list_claude_cards` during this very session); `docs` for the Projects knowledge base specifically, which Dave hasn't used. See cards 007, 008.
**Relevant to:** 2 (operating Claude products), 4 (integrating — MCP and APIs), 5 (building with Claude Code)

## Content

Every context mechanism sits somewhere on one axis: **is the knowledge already in the window, or does the model have to go get it?**

| | **Passive context** | **Active retrieval** |
|---|---|---|
| Examples | Project knowledge base, CLAUDE.md, pasted text | MCP tools, file reads, web search |
| Presence | Guaranteed — it's there every turn | Conditional — only if the model decides to look |
| Freshness | Snapshot; stale until refreshed | Live at call time |
| Corpus size | Bounded by window (or auto-RAG) | Effectively unbounded |
| Cost | Paid every turn, whether used or not | Paid per lookup, only when used |
| Precision | High, if curated | Depends on the query the model guesses |

### The failure modes are opposites, and that's the whole point

- **Passive fails by dilution.** Everything is present, so nothing stands out. A knowledge base full of marginally-relevant material makes the relevant part harder to find, and you pay for it on every single turn.
- **Active fails by omission.** The material is perfect and current, but the model never thought to call the tool — or called it with a query that missed. The knowledge might as well not exist.

That second failure is the sneaky one, because nothing looks wrong. There's no error, just an answer produced without consulting material that would have changed it.

### The hybrid is usually the right answer

Put a **small passive pointer** in context that tells the model when to retrieve actively. A few lines in CLAUDE.md or a project instruction — *"Dave's reference cards on Claude tooling are available via `list_claude_cards`; check them before answering questions about MCP, Claude Code workflows, or context management"* — converts an omission failure into a decision the model can actually make.

This is also why an MCP tool *description* carries so much weight: it's the passive advertisement for an active capability. A vague description means the tool never fires.

## Why this matters

Nearly every "how should we set up context" question resolves once you ask which pole the situation needs — and the answer follows from two properties of the material: **how often it changes** (freshness pressure → active) and **how reliably it must be consulted** (omission intolerance → passive).

It also explains why the Projects-vs-CLAUDE.md comparison felt like it was about surfaces when it was really about access patterns. Same axis, different products.
