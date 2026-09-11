# CLAUDE.md — Working Instructions

This file tells Claude how to collaborate on this project. If you're a human, see the README for what this is.

---

## What This Project Is

A hands-on curriculum for people leading Claude adoption: center-of-excellence leads inside companies, and consultants/systems integrators who integrate Claude with client systems — with an emphasis on Salesforce.

It is also **Dave's own learning vehicle**. A large share of the material covers Anthropic products and surfaces he does not use day to day. That is the point: the curriculum is built by learning the thing, doing it, and writing down what actually happened.

## What We're Actually Building (read this before optimizing anything)

**The cards are the product. Everything else is a rendering of them.**

The real work is building a knowledge primitive — a repeatable way to create, document, and synthesize what's been learned. The boot camp is one output. A conference deck, an MCP server, a book, a client workshop are others. All of them get generated later from whatever has accumulated.

So: **do not optimize for an output format up front.** If a decision makes the cards better but the boot camp slightly harder to assemble, make the cards better. Format is downstream and cheap; the underlying material is the expensive part.

## Working Style

**Iterative and tangent-following, not linear.** Follow whatever topic is interesting or useful right now, document it, move on. Do not work through the six sections in order, and do not build things for the sake of completeness.

- **For products Dave already knows well** (Claude Code, Claude.ai/chat): mostly carding existing knowledge — get what's in his head onto cards.
- **For products he uses less**: approach as a consultant/architect first. Ask what the use cases and the actual value are, and use that to decide whether it's worth implementing at all. Not everything deserves a lab.

**Starting point:** Claude Code and Claude.ai/chat — the two with the most hands-on experience — then follow tangents from there.

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

## Don't Recreate What Already Exists

[Anthropic Academy](https://anthropic.skilljar.com) is good, and it covers the lecture register well — high-level, conceptual, not hands-on. There is no point rewriting it.

So where a topic's **lecture** beat is already covered well somewhere else, the lecture is allowed to be a **pointer**: "go read/watch X — here's why it matters, and here's what it means for your situation." The card records that it was learned by reading or watching rather than running (`Verified: docs`).

**Labs stay original.** They have to be — a lab is by definition something actually run, and that's the part no existing course provides. The value this project adds is the hands-on half plus the judgment about what's worth doing at all, not a re-narration of the concepts.

## Dave Dictates — Expect Transcription Errors in Proper Nouns

Much of Dave's input is voice-to-text. Speech recognition handles the prose fine and mangles exactly the words that matter most: product names, file names, model names. **Silently correct these and keep going — don't ask about an obvious mistranscription.**

| He means | Often appears as |
|---|---|
| Claude for iOS | Quadfly OS, Claude Freyos, Cloud fly OS |
| Claude Code | quiet code, cloud code |
| CLAUDE.md | Claude MD, Claude dot markdown, cloud dot MD |
| `<anything>.md` | "\<anything\> dot markdown" |
| Sonnet / Opus / Haiku | sonic, opis, haiku (usually fine) |
| MCP | MCP, empty P, M C P |
| Anthropic | anthropic, and thropic |
| Salesforce / Heroku | sales force, heroic |
| Orginator | originator, orchestrator |

Add rows as new ones show up — this is meant to grow. If a term is going to be said constantly and transcribes badly, consider *renaming the thing* (the book's sessions are Alpha/Beta/Charlie precisely because NATO-alphabet names survive dictation).

When a mistranscription is genuinely ambiguous and the choice changes what you'd do, ask. When context makes it obvious, just fix it.

See card 006 for the full treatment, including preprocessing options.

## Learning Inputs

Dave often consumes documentation **while walking**, via [Eleven Reader](https://elevenreader.io) (import a URL or paste text, listen on the go). No pipeline needed for casual use. If batch listening ever becomes worth automating: scrape doc pages to markdown and either hit the ElevenLabs API per page, or reuse the local Piper TTS endpoint (`POST /tts`) from the `conduct` repo for a free/local option at lower voice quality.

Relevant to carding: **listening to docs is `docs`, not `ran-it`.** Audio is an input channel, not verification.

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

## Two Sessions, Two Jobs

Mirrors the book's Alpha/Beta split:

- **Seminar chat (Claude.ai)** — the professor / synthesis partner. Where Dave learns out loud, works through a product, argues with the material, and decides what's worth carding. Exploratory.
- **Claude Code session** — repo work. Writing the cards to disk, building and running labs, wiring up rendering, and generating the final outputs (boot camp write-up, MCP, deck).

Synthesized material moves from the seminar chat to the Claude Code session as planning notes. When that handoff arrives, fold the durable decisions into this file and the repo rather than leaving them in a transcript.

## Relationship to the Website

This repo is pulled into [davestanton.com](https://davestanton.com) as a git submodule at `content/claude/` and rendered at `/claude`. Content is authored **here**; the website session only pulls and publishes. Do not author curriculum content from the website repo.
