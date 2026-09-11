# Ad-Hoc Artifact Reports as an Answer to Salesforce Custom-Report Sprawl — Gists for Org Data

**Source:** Dave, Claude Boot Camp session 2026-09-11, putting a Salesforce hat on the artifacts capability. The gist analogy is Beta's, same session.
**Type:** preference
**Verified:** `inferred` — a proposed workflow, not yet built. Underlying artifact mechanics are `docs`-verified (cards 028–030). `[confirm-this: build one end-to-end against a real org and see whether the SOQL/analysis loop is actually faster than the custom report it replaces.]`
**Relevant to:** 6 (Claude + Salesforce), 4 (integrating — MCP), 2 (operating Claude products)

## The problem it addresses

Salesforce orgs accumulate **custom-report sprawl**: hundreds of saved reports, most built once to answer a question someone had on a Tuesday, all of them now permanent objects someone has to maintain, name, and find. The alternative for a one-off question is writing ugly SOQL and eyeballing the results — or filing a report request and waiting.

Most of those reports never needed to be permanent. They needed to be **an answer, once, shareable.**

## The workflow

1. **Chat with the data** through a connector — Salesforce or another datastore — including from the phone. Claude does the reasoning the query language is bad at: cleansing, bucketing, judgment calls about what counts.
2. **Get to the answer conversationally**, iterating, instead of encoding the whole question in SOQL up front.
3. **Publish the result as an artifact** — a visualization, a cleaned table, or the cleansed SOQL itself.
4. **Share the link** for org-scoped collaboration.

The analogy is a **GitHub gist**: versioned, single-purpose, link-shareable, and explicitly *not* trying to be a governed system of record. It fills the gap between "I need a real answer right now" and "I need to file a request."

The real gain is in step 1. Bucketing, fuzzy categorisation, "which of these look like duplicates," "group these by something that isn't a field" — these are cumbersome in SOQL and natural in conversation. Getting a reasoned, visualised answer in five minutes beats both the ugly query and the new permanent report.

## Design decisions this forces

**Pick the data mode deliberately (card 030).** This is the whole governance question:

- **Embedded snapshot** — the numbers are pulled under *your* field-level security. Correct for "here's what I found, point in time," and it **must** be shared org-scoped, never public. You are the access control, and CRM-derived data one link from world-readable is exactly the failure you don't want.
- **Connector-backed** — each viewer's own access governs what they see, so the permission model you were careful about (card 024) survives the sharing step. Costs: every viewer needs the connector on their claude.ai account, and it can't be public-linked on any plan.

**Say which one it is when you share.** A link that looks like a dashboard will be treated as one. If it's a point-in-time pull, title it that way — otherwise someone quotes last week's numbers in a meeting and the artifact gets blamed for being wrong when it was only being honest.

**Team/Enterprise, not Pro/Max**, for anything org-derived. On Pro/Max a public link is the *only* sharing option (card 028) — which for CRM data means the option is effectively "don't share it."

## Why this matters

It reframes a class of work. "Build me a report" currently means creating a durable object in a governed system, with all the naming, permissions, and maintenance that implies. A lot of those requests are really *questions*, and treating them as ephemeral artifacts keeps the org clean while answering faster.

For consultants and SIs it's also a demo with unusually little setup: a connector, a question, and a link — showing value without touching org configuration at all, which is exactly the pitch that gets you past a cautious admin.

The caution that belongs alongside the pitch: **semi-ephemeral is a discipline, not a feature.** Nothing expires these automatically, and a gist that becomes load-bearing is just an ungoverned report. If a link gets used repeatedly, that's the signal it should graduate into a real report in the platform.
