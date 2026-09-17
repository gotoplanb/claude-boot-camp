# Artifacts Have Two Data Modes — an Embedded Snapshot Carries *Your* Permissions, a Connector-Backed Page Uses *Theirs*

**Source:** Claude Boot Camp session 2026-09-11, correcting two caveats that were stated backwards. Verified against [Share session output as artifacts](https://code.claude.com/docs/en/artifacts) → *Pull live data with MCP connectors*.
**Type:** concept
**Verified:** `docs` — read in full. **Not yet run.** `[confirm-this: build one of each and compare refresh behaviour and what a second viewer sees.]`
**Relevant to:** 4 (integrating — MCP and APIs), 6 (Claude + Salesforce), 3 (administering)

> **Scope:** this card is the **mode choice** and its governance consequences. The viewer-execution mechanics of the connector-backed mode — per-viewer prompts, side-effect attribution, sharing limits — belong to card 029.

## The two modes

An artifact gets its data one of two ways, and **they are opposites on both axes that matter**:

| | **Embedded snapshot** (default) | **Connector-backed** (ask for it) |
|---|---|---|
| How data gets in | Claude queries while building; results are baked into the page | The page calls MCP connectors **each time someone views it** |
| Freshness | Frozen at build time | Live on load; can refresh on an interval or a control |
| Whose permissions | **The author's** — pulled under your access | **Each viewer's own** — their account, their connectors |
| Everyone sees | The same thing | Possibly **different** things |
| Public link | Allowed | **Not allowed on any plan** |

You get the snapshot by default. You get the connector-backed page by asking for it — *"pull the live list through my GitHub connector when the page loads."*

## The correction

Two caveats commonly attached to artifacts are true of the **snapshot** mode and **false** of the connector-backed one:

- ❌ *"It's a snapshot, not a live dashboard — it won't re-query on refresh."* True by default, **false** for connector-backed pages, which exist precisely to avoid that. The docs: the page shows *"current data rather than a snapshot from the session that built it."*
- ❌ *"Security context travels with whoever generates it, not whoever views it."* Exactly backwards for connector-backed pages — calls run under the **viewer's** account (card 029). It's the *embedded* mode where the author's access is baked in.

Getting this backwards matters because it inverts the risk. The mode people assume is safe (a static page) is the one that can leak; the mode that sounds riskier (live queries on someone else's behalf) is the one that preserves the permission model.

## The permission consequence, stated plainly

**Embedded snapshot = a permissions bypass waiting to happen.** The data was pulled under *your* field-level security and permission sets. Share it with someone whose profile shouldn't see those fields, and they see them anyway — in a link, outside the platform's access model entirely. For CRM-derived data this is the real hazard, and it's invisible: nothing errors, the page just renders.

**Connector-backed = the permission model survives.** Each viewer's own access governs what renders (card 029 has the mechanics).

So the guidance inverts by mode:

- **Sharing an embedded snapshot** → treat it as an export. Org-scoped sharing only; never a public link if the data came from a governed system. You are the access control.
- **Sharing a connector-backed page** → the platform is the access control, but every viewer needs the connector on their **claude.ai account** — a local `.mcp.json` server can supply data while Claude *builds* the page and cannot be called by the published page.

## Why this matters

Choosing the mode is a **governance decision disguised as a formatting choice**. Nothing in the authoring flow announces that you've just moved data outside its permission model, and the default is the leaky one.

For anyone advising on adoption, this is the artifact question worth asking first: *where does the data come from, and under whose access?* Everything else about artifacts — freshness, sharing scope, what a viewer sees — follows from the answer.
