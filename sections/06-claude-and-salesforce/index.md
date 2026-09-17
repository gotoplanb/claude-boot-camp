---
title: "6. Claude + Salesforce"
order: 6
status: drafting
---

## Scope

The integration path in depth, for consultants and SIs.

**The section's honest state:** ten cards claim it — second-most in the corpus — but only five are `ran-it`, and they cover *gotchas and boundaries*, not the section's headline promise. The material for "chat plus MCP against your org" is carded from 031, 032, and 034, which are `inferred` or `docs`. **The section can lecture its core claim. It cannot yet demonstrate it.**

That gap is one artifact wide. See the gaps section.

---

## Topic 6.1 — What Salesforce doesn't tell you it did

*The strongest, best-verified hour in the section.*

### The spine

**"Created" and "usable" are different states**, and the tooling reports success on the first while you needed the second.

### Lecture arc

**1. The configuration layers aren't in the API (card 024, `ran-it`).**
A custom object is created and invisible until you also create a tab, assign it to applications, and set field-level security. The `sf` CLI truthfully reports success on incomplete work. Nothing errors. The object simply isn't there when a user looks.

This is the perfect opening for a Salesforce audience because they've all been bitten by it, and it reframes the whole section: the risk isn't that Claude writes bad Apex, it's that the platform has invisible state the model can't see either.

**2. Claude gets a disposable org. Always (card 026, `ran-it`).**
*"At least until you've built the gotcha corpus."* The `sf` CLI is powerful enough to damage production. A scratch org makes a mistake cost a rebuild instead of an incident.

**3. Stay outside the vendor's contracts (card 050, `ran-it`).**
Don't write into a managed package. It's not yours, upgrades will break you, and the failure arrives at the worst possible time — during someone else's release.

**4. Mining the gotchas is itself the deliverable (card 025, `ran-it` technique).**
Solve a challenge manually, keeping notes. Hand Claude only the *notes* and a clean org. Diff the two orgs. **The gaps in your own notes are the tacit knowledge** — the things you knew so well you didn't write them down.

Be precise about what this can and can't do: platform-structural gotchas (FLS before visibility, tab assignment, permission-set sequencing) surface reliably. Org-specific accumulated weirdness — the actual long tail of consulting — **cannot** be surfaced this way by construction.

### Lab

Complete the configuration chain by hand — create, tab, assign, FLS — observing invisibility at each step. Then ask Claude to do it and watch it stop at "created." Requires a scratch org or Developer Edition. Roughly 30 minutes and entirely `ran-it`.

### Takeaway

Before trusting a deployment, ask what the platform requires that the API never mentioned.

---

## Topic 6.2 — Chat plus MCP against your org

*Draftable as a lecture. Not yet demonstrable.*

### The spine

Pre-built views don't scale to ad-hoc questions; a generated-per-question view does.

### Lecture arc

**1. The structural argument (card 032, `inferred`).**
Salesforce Mobile has no layout for *"who do I actually need to call on this account?"* — and never will, because the question wasn't anticipated. This is a structural limit of designed-in-advance views, not a polish gap. Three honest limits: it can't see what isn't logged, latency is real, and non-determinism applies.

**2. Ad-hoc artifacts against report sprawl (card 031, `inferred`).**
Orgs accumulate hundreds of custom reports, most built once to answer a Tuesday question and now permanent objects someone maintains. *"Most of those reports never needed to be permanent. They needed to be an answer, once, shareable."* The gist analogy: versioned, single-purpose, link-shareable, explicitly not a governed system of record.

**3. Pick the data mode deliberately (card 030, `docs`).** Embedded snapshot pulled under your FLS — must be org-scoped, never public, for CRM data. Connector-backed — each viewer's own access governs. See §2.2.

**4. Semi-ephemeral is a discipline, not a feature.** Nothing expires these. A gist everyone depends on is just an ungoverned report. Repeated use is the signal to graduate it into the platform.

**5. Salesforce productised the pattern (card 034, `docs`).**
Claudeforce ships prebuilt sales skills as a first-party integration. Easiest sell; the DIY route keeps the advantage of scaling to arbitrary objects and unanticipated questions. **Volatile card** — announced mid-rollout, pricing unpublished.

### Lab

**None available.** Every card this hour rests on is `inferred` or `docs`. Under the project's rule, no lab can be built until someone runs it.

---

## Topic 6.3 — Modelling and boundaries

Supporting hour, from `ran-it` material.

- **Separate the dimensions (card 051, `ran-it`).** Price book apart from credits book; derive consumption rather than storing it. Built, and it caught four real defects.
- **Determinism where it will be questioned (card 033, `inferred`).** Rollups for anything that drives a decision; Claude for discernment.

---

## What's verified and what isn't

The most `inferred`-heavy section in the corpus. Full list, because teaching any of these as fact would be a mistake.

| Claim | Card | Status |
|---|---|---|
| Harvest a click-path in a sandbox, ship the script | 063 | `ran-it` — standing method |
| A harvested API sequence may skip UI-fired logic | 064 | `inferred` — **nobody has hit it**; teach as caution |
| Partner motions need identity, not a chat box | 062 | `inferred` — an untested product idea |
| Configuration layers invisible to the API | 024 | `ran-it` |
| Disposable org for Claude | 026 | `ran-it` |
| Don't write into managed packages | 050 | `ran-it` |
| Separate price book from credits book | 051 | `ran-it` |
| Notes-and-diff elicitation | 025 | `ran-it` technique, `inferred` protocol — `[confirm-this: run the full protocol once end-to-end and record how many gotchas the diff surfaces that the notes missed.]` |
| Ad-hoc artifact beats a custom report | 031 | `inferred` — `[confirm-this: build one end-to-end against a real org and see whether the SOQL/analysis loop is actually faster than the custom report it replaces.]` |
| Chat + MCP as primary mobile surface | 032 | `inferred` — `[confirm-this: use it as the primary mobile Salesforce surface for a week and record where it actually beat the app and where it didn't.]` |
| Artifact data modes behave as documented | 030 | `docs` — `[confirm-this: build one of each and compare refresh behaviour and what a second viewer sees.]` |
| Claudeforce capabilities | 034 | `docs` — mid-rollout, ages fast |
| Salesforce-in-Claude will be metered | 034 | **prediction**, explicitly Dave's bet |
| Trust machinery bundled into metered price | 036 | `inferred` — *"not something I could confirm in the pricing docs"* |
| Determinism buys debuggability | 033 | `inferred` |

---

## Gaps — where more hands-on time is needed

**1. The keystone: build and publish a real Salesforce MCP connector.** One artifact — query Accounts and Opportunities, render the result in a connector-backed artifact, run it against a real org — converts cards 030, 031, and 032 from `inferred` to `ran-it`, unlocks the only lab for topic 6.2, and gives the pattern cards a reference implementation. **Nothing else in this section comes close to the same leverage.** Everything in 6.2 leans on it.

**2. Run the notes-and-diff protocol end to end** (boot camp issue #5). Upgrades card 025's protocol and produces genuine new gotcha cards as a by-product.

**3. No governed write pattern.** Card 050 says what not to write to. Nothing says what Claude *should* write to, or where the human gate belongs between dev and production.

**4. No production-safe CI/CD boundary.** 024 and 026 teach you not to break things; neither shows the pipeline shape that keeps it that way.

**5. Cost comparison is unbuildable right now.** Cards 034, 035, and 036 are all inferred or predicted, and Salesforce-in-Claude pricing is unpublished. Don't teach the comparison until it resolves.

**6. No superbadge/certification guidance.** Card 025 leans on superbadges as the elicitation source without saying which ones are worth mining, or which certifications matter to a Salesforce CoE. That's a go-to-market question this audience will ask.
