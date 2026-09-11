# CLAUDE.md — Working Instructions

This file tells Claude how to collaborate on this project. If you're a human, see the README for what this is.

---

## What This Project Is

A hands-on curriculum for people leading Claude adoption: center-of-excellence leads inside companies, and consultants/systems integrators who integrate Claude with client systems — with an emphasis on Salesforce.

It is also **Dave's own learning vehicle**. A large share of the material covers Anthropic products and surfaces he does not use day to day. That is the point: the curriculum is built by learning the thing, doing it, and writing down what actually happened.

## Teaching Model

Every topic is roughly **one hour**, in three beats:

1. **Short lecture** — establish the why and the context. What is this, what problem does it solve, when would you reach for it.
2. **Lab / live implementation** — build it in front of the audience. Real commands, real failures, real output.
3. **Takeaway** — homework so the participant does it themselves.

**The demo is one path, not the path.** Dave's way of working is not how everyone will be most productive. The takeaway exists so each participant finds their own. Never write a lab that implies there is one correct workflow.

## The Card-First Method (borrowed from the book)

This project follows the same method as *Geography as Destiny*: **accumulate reference cards first, synthesize into prose later.**

- Cards are filed **as encountered**, not organized by section up front. Categorization happens during synthesis, when the shape is known.
- Expect on the order of **100+ cards** before the first serious rewrite of a section.
- A card is a single idea, demo, gotcha, product behavior, or pattern.
- When there are enough cards in an area, the section gets drafted/rewritten from them. Sections are **continually rewritten** as cards accumulate — they are never "done" early.

Do not try to write polished section prose before the cards exist. That produces confident, plausible, unverified curriculum — the exact failure this method is designed to prevent.

## Verification Discipline (the most important rule)

Because much of this covers products Dave hasn't used yet, **every card must be honest about how it was learned.** Each card carries a `Verified:` field:

- `ran-it` — Dave (or Claude, in a real session) actually executed this and observed the result. Highest confidence.
- `docs` — read in official documentation, not yet run. Plausible but unconfirmed in practice.
- `inferred` — reasoned from related behavior. Lowest confidence. Must be confirmed before it reaches a lab.
- `confirm-this` — explicitly flagged as a question to resolve.

**A lab may only be built from `ran-it` cards.** Teaching something you haven't run is how a curriculum gets confidently wrong in front of a room. If a lab needs a step that is only `docs` or `inferred`, run it first and upgrade the card.

Mark uncertainty inline as well, in the body: `[confirm-this: …]`.

## Naming and Numbering

- Cards: `cards/NNN-short-description.md`, numbered sequentially as encountered. The number is for uniqueness only; the description is for scanning. Do **not** renumber to group by topic.
- Sections: `sections/NN-slug/index.md`. Six to start (see `curriculum.md`).
- Labs: `labs/<slug>/` — runnable, self-contained, with a README stating prerequisites and expected output.

## Sections (starting structure)

1. Foundational AI and concepts
2. Operating Claude products
3. Administering Claude products
4. Integrating with Claude (MCP and APIs)
5. Building with Claude Code
6. Claude + Salesforce

These are a starting point, not a commitment. Follow the evidence: if the cards say a section should split, merge, or reorder, say so.

## Audience — write for these two readers

- **CoE lead** — standing up and running the Claude function at a company. Cares about rollout, standards, governance, spend, and demonstrating results.
- **Consultant / SI** — integrating Claude with systems a client already runs, especially Salesforce. Cares about what actually works, what to avoid, and what's deliverable.

If a card or section serves neither reader, it probably belongs in Dave's blog instead.

## Voice

Match the voice of davestanton.com:

- Direct openings, no throat-clearing.
- Short paragraphs. Concrete over abstract — real commands, real numbers, real file paths.
- Define by what it's NOT; scope by exclusion.
- No marketing speak, no hype, no emoji. Ever.
- Honest about limits. "I haven't run this yet" is always better than implied authority.

## Relationship to the Website

This repo is pulled into [davestanton.com](https://davestanton.com) as a git submodule at `content/claude/` and rendered at `/claude`. Content is authored **here**; the website session only pulls and publishes. Do not author curriculum content from the website repo.
