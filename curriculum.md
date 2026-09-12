# Curriculum — Working Outline

Six sections. Each topic is ~one hour: lecture, lab, takeaway.

This is a **starting structure, not a commitment**. Sections will split, merge, and reorder as cards accumulate. Topic lists below are seeds — they get replaced by what the cards actually support.

Status values: `outline` (structure only) · `carding` (cards accumulating) · `drafting` (prose being written from cards) · `taught` (run with a real audience)

---

## 1. Foundational AI and concepts — `outline`

The vocabulary and mental models everything else depends on. Enough grounding to make good decisions, not a machine-learning course.

Seeds: what these models do and don't do · context and why it runs out · tokens and cost structure · where models fail and how failure looks · evaluation, and why "it seemed good" isn't one · when not to use AI at all.

## 2. Operating Claude products — `outline`

Using the products well, day to day. The surfaces, what each is good at, and the habits that separate a productive session from a chaotic one.

Seeds: the product surfaces and when to reach for each · Projects and persistent context · organizing work that spans days · getting useful output from a bad first answer · sharing work with a team.

## 3. Administering Claude products — `outline`

The center-of-excellence job. What you own once more than a handful of people are using this.

Seeds: provisioning and access · plan and tier differences that actually matter · policy and acceptable use · spend visibility and controls · rollout to a skeptical org · measuring whether it's working.

## 4. Integrating with Claude (MCP and APIs) — `outline`

Exposing your own systems and content as tools Claude can call.

Seeds: the API, in practice · what MCP is and when it beats a plain integration · building a small MCP server · auth for real clients · connecting an existing internal system · what to expose and what to withhold.

## 5. Building with Claude Code — `outline`

From an empty repo to a deployed, tested artifact.

Seeds: setting up a workspace that stays sane · the full loop — build, test, deploy · project instructions and why they matter · agents and workflows · where to draw the line between what you automate and what you keep · reviewing work you didn't write.

## 6. Claude + Salesforce — `outline`

The integration path in depth, for consultants and SIs.

Seeds: org data and metadata access · the Salesforce MCP surface · deployment and test workflows · what works, what to avoid · packaging this as a client deliverable.

---

## Method

Cards first. See [`cards/README.md`](cards/README.md). A section moves from `outline` to `drafting` only when there's enough carded material to write from — not before.

**Three teaching instruments, not two.** Lecture establishes the why, the lab proves one mechanism in an hour — and **extended case studies** show what months of compounding decisions produce, which is the thing participants are actually aiming at and can't experience in a session. Case studies are the closing exemplar for a section and the prompt for a participant's own go-to-market plan. See card 049; the first one is the brokerage build.

Sections are **not worked in order**. Starting with Claude Code (5) and the Claude apps (2), where the hands-on experience already exists, then following tangents. See [`CLAUDE.md`](CLAUDE.md) for the working style, and the cards for the method and stance.

Where a lecture beat is already covered well elsewhere — [Anthropic Academy](https://anthropic.skilljar.com), official docs — the lecture can be a pointer rather than a rewrite. Labs stay original.
