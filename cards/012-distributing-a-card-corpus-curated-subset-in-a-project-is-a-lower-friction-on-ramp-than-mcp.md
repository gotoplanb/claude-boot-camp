# Distributing a Card Corpus — A Curated Subset in a Project Is a Lower-Friction On-Ramp Than Connecting an MCP Server

**Source:** Dave, Claude Boot Camp session 2026-09-11: "today just find a couple cards that you find interesting and throw those into a project and you instantly have a more domain knowledgeable chat."
**Type:** concept
**Verified:** `ran-it` for the MCP path — the corpus is live at davestanton.com/mcp and was queried from a Claude app chat during this session. `docs` for the Project path — the subset-upload flow hasn't been tried yet. `[confirm-this: how well does a 5–10 card upload actually steer a chat? Needs a real trial.]`
**Relevant to:** 2 (operating Claude products), 4 (integrating — MCP and APIs)

## Content

Once a reference-card corpus exists, there are two ways for someone (including its author) to put it to work in a chat. They differ mostly in **friction to first value** and in the passive/active split from card 011.

### Path A — download a subset, drop it in a Project

Pick the handful of cards relevant to a topic, upload them as markdown into a Claude Project, start chatting. The cards sit passively in context for every chat in that project.

- **Friction:** near zero. No install, no auth, no server. Works today for anyone who can download a file.
- **Focus:** high. A curated five-card subset steers hard, because everything present is relevant.
- **Best for:** the many-one-off-chats pattern — brainstorming, customer discovery, working a single topic over days. Each project is a purpose-built domain expert.
- **Cost:** it's a snapshot. It goes stale as cards change, and refreshing is manual.

### Path B — connect the MCP server

Point a client at the live server; every chat can reach the whole corpus via `list_claude_cards` / `read_claude_card` / `search`.

- **Friction:** real. Install or connector setup, auth, and a client that supports it.
- **Coverage:** total, and always current — it reads live off the site.
- **Precision:** depends on the model choosing to search and guessing a good query. See card 011's omission failure.
- **Best for:** the author, and anyone who wants the corpus available everywhere without per-topic setup.

### The recommendation

**Lead with Path A for other people.** "Grab two or three cards that look interesting, drop them in a project, and you instantly have a more domain-knowledgeable chat" is a complete first experience with no setup. Path B is the power-user upgrade, offered second — *you totally can connect the server, and here's how* — rather than as the price of entry.

For the author, both: MCP everywhere for retrieval, plus purpose-built projects when a topic deserves a focused, passive context.

## Implication worth building

If Path A is the on-ramp, the corpus should be **easy to take a subset of** — per-card raw markdown, and ideally a way to select several and download them together. Without that, "grab a few cards" means copy-pasting out of a web page, which is enough friction to lose people at exactly the moment the pitch is *low friction*.

`[confirm-this: does davestanton.com/claude need a download affordance — per-card raw markdown, and a multi-select bundle?]`

## Why this matters

This is the distribution question for any knowledge corpus, and the instinct is to lead with the impressive integration. That's backwards: the impressive path has setup cost, and setup cost is where people quit. The boring path — download three files, upload three files — delivers value in under a minute and makes the case for the integration far better than a pitch would.
