# Skill, Command, Connector, Plugin — Four Things, One of Which Holds the Credentials

**Source:** Dave in Claude for Mac, 2026-09-17, installing Anthropic's **Data** plugin and finding both a "Data" plugin and a data skill in Customize: *"are they like the same thing?"* Beta confirmed the answer against its own live skill list, same session.
**Type:** product-behavior
**Verified:** mixed, deliberately — see the table below.
**Relevant to:** 2 (operating Claude products), 4 (integrating), 5 (building with Claude Code), 1 (foundational concepts)

## What's verified and what isn't

| Claim | Status |
|---|---|
| The Data plugin registers **ten** skills, all namespaced `data:` | `ran-it` — Beta enumerated them from its own live skill list |
| The `data:` prefix marks plugin provenance and avoids name collisions | `ran-it` — visible in the names |
| Slash commands are **not** 1:1 with skills (`data-context-extractor` has no matching command) | `ran-it` |
| A plugin is `plugin.json` + `.mcp.json` + `commands/` + `skills/` | `docs` — *[confirm-this: stated from knowledge of the structure, not read off a page in this session. Link the plugin reference.]* |
| "About six" slash commands | **approximate — not counted.** Don't quote a number until someone counts them |
| A skill is "plain markdown, no code" | `inferred` — *[confirm-this: this is the common case, but skills can carry supporting files. Verify before teaching "markdown only" as a definition.]* |

## The four things

**Skill** — an instruction file Claude loads into context **when it decides the skill is relevant**. You don't invoke it; it fires on its own. This is the progressive-disclosure mechanism priced in card 059.

**Command** (slash command) — the **explicit-trigger** counterpart. `/analyze`, `/write-query`, `/build-dashboard`. You call these on purpose.

> Skills and commands are the same idea split by *who decides*: the model decides a skill is relevant, you decide a command should run.

They're related but **not a strict 1:1**. Some skills have no slash command and are reached by asking in natural language instead — `data-context-extractor` is the observed example.

**Connector** — the OAuth/MCP wiring to an external system (Snowflake, Databricks, BigQuery). **This is the only one of the three that holds real credentials and reaches live data.**

That asymmetry is the load-bearing part. Skills and commands are inert without it — still usable by pasting in a CSV or query results by hand, but nothing gets queried. If you're asking "what can this actually touch," the answer is always the connector, never the skill.

**Plugin** — the packaging layer above all three. A manifest plus connector wiring plus a commands folder plus a skills folder, installed as **one unit**.

## Answering the original question

The plugin and the skill are not the same thing: **the plugin is the container, the skill is one of ten things inside it.**

The Skills tab wasn't showing a duplicate. It was showing the individual components the plugin installed — which is also why they're all prefixed `data:`.

Installing "Data" didn't add one thing called Data. It added ten skills (`data:analyze`, `data:data-visualization`, `data:build-dashboard`, `data:write-query`, `data:statistical-analysis`, `data:sql-queries`, `data:create-viz`, `data:explore-data`, `data:validate-data`, `data:data-context-extractor`), a set of slash commands, and connector definitions for three warehouse types — in one install instead of assembling each piece by hand.

**This also refines card 037**, which describes plugins as "bundles of skills." They bundle commands and connector wiring too, and the connector is the part that matters most for access.

## Bundle or cherry-pick — a real cost, not a rhetorical aside

You could install just the data-visualization skill standalone rather than the whole plugin.

That's worth weighing, because of card 059: **every installed skill costs its listing in context every session**, used or not. Ten skills' worth of listings is a small standing tax, but it's real and it's permanent — and if you installed the plugin for one of the ten, you're carrying nine you'll never fire.

- **Bundle** when the plugin's domain matches something you do repeatedly, and you'd plausibly reach several pieces.
- **Cherry-pick** when you wanted one capability and the rest is someone else's workflow.

The convenience of one install is genuine. It just isn't free, and the cost lands on every session afterwards rather than at install time — the same "you pay at connect time, not use time" shape card 059 describes for MCP tools.

## Why this matters

These four words get used loosely and they are not interchangeable. The distinction that matters operationally is **which one holds credentials** — you can audit a skill by reading it, and you cannot audit what a connector reaches without checking the account behind it (cards 046, 065).

It also makes plugin installs legible: a plugin is not one capability, it's a bundle whose contents you should look at before installing, because they all land in your context and one of them may be wiring to a live warehouse.

See card 059 (what skills and tools cost in context), card 060 (plugin marketplaces as org-internal distribution), card 037 (access and instructions travelling together — refined here), cards 046/065 (whose permissions and whose account a connector uses), and card 066 (whether to connect at all).
