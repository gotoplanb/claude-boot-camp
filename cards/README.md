# Cards

The raw material of the curriculum. One idea per file, filed **as encountered** — not organized by section. Organization happens later, during synthesis.

Expect 100+ of these before a section gets seriously written.

## File format

```markdown
# [Short descriptive title]

**Source:** [where this came from — docs URL, a real session, a failure you hit, a conversation]
**Type:** concept | product-behavior | demo | gotcha | pattern | integration | preference | decision | question
**Verified:** ran-it | docs | inferred | n/a | confirm-this
**Relevant to:** [section number(s) or "general"]

## Content

[The thing itself. Commands, output, the behavior, the idea.]

## Why this matters

[Why it earns a place in the curriculum — what it lets someone do, or what it saves them from.]
```

## Cite your sources — link them

**If a claim came from documentation, link the documentation.** A reader six months from now needs to check whether it's still true, and a bare assertion gives them nowhere to start.

Put links in the `Source:` line, inline in the body where a specific claim needs backing, or both:

```markdown
**Source:** [Anthropic — hooks reference](https://code.claude.com/docs/en/hooks.md),
verified 2026-09-11. Behaviour confirmed against the "what survives compaction"
table in [memory.md](https://code.claude.com/docs/en/memory.md).
```

**Cite what you actually read, not what you assume backs it.** If the claim came from a cached reference, a subagent's research pass, or a conversation rather than the live page, say so and *also* link the canonical source. "Verified against the docs" when you actually read a summary is the kind of small dishonesty that makes the whole `Verified` field untrustworthy.

This matters most for the distinction Dave keeps asking about: **is this an official product behaviour, an architecture pattern, or someone's preference?** A link answers that instantly. No link, `Type: preference` — those are opinions and should read as such.

## Opinion vs. fact — the distinction that matters most

Expect roughly **half these cards to be opinions**: Dave's preferences, working style, and "best practices." The other half are verifiable — code examples, product behavior, specific implementation details.

Those two must never be confused, because presenting a preference as a fact is precisely what the project's stance refuses to do (*a way, not the way*). The head matter keeps them apart:

- **`Type: preference`** — how Dave chooses to work. Someone else could reasonably do the opposite and be fine. Write these in first person and own them as opinion.
- **`Type: decision`** — a settled position or project stance. Nothing to verify.
- Everything else (`product-behavior`, `gotcha`, `demo`, `integration`, …) is a **claim about the world** and must be verifiable.

## The Verified field

The `Verified:` field records how a *factual* claim is known — and stays honest about the fact that much of this curriculum covers products that hadn't been used day-to-day when the card was written.

| Value | Meaning |
|---|---|
| `ran-it` | Actually executed and observed. Highest confidence. |
| `docs` | Read in official documentation, not yet run. |
| `inferred` | Reasoned from related behavior. Lowest confidence. |
| `n/a` | Nothing to verify — a `decision`, or a pure matter of taste. |
| `confirm-this` | An open question to resolve. |

### The two fields combine, and the combination carries the meaning

A preference can still be backed by execution, and that difference matters:

| Head matter | Reads as |
|---|---|
| `preference` + `ran-it` | A battle-tested habit — Dave has worked this way for real. Weight it. |
| `preference` + `inferred` | An untested idea he likes the sound of. Try it, don't teach it. |
| `preference` + `docs` | A recommendation he's read but not adopted yet. |
| `decision` + `n/a` | A stance. Agree or don't; there's nothing to check. |
| `gotcha` + `ran-it` | A fact. He hit this. |

### When one card's claims differ in confidence

Usually every claim in a card sits at the same confidence, so one `Verified:` value covers it. When it doesn't — a checked fact, a reasonable guess, and an outright bet in the same card — put a small table near the top marking each claim:

```markdown
## What's verified and what isn't

| Claim | Status |
|---|---|
| Agentforce meters at $0.10/action | `docs` — current rate card |
| Trust machinery is bundled into that price | `inferred` — couldn't confirm |
| The new product will also be metered | prediction — Dave's bet, not a claim |
```

Set `**Verified:** mixed, deliberately — see the table below.` and let the table carry it. Card 036 is the worked example.

The reason to bother: without it, a shaky claim borrows credibility from a solid one sitting next to it. Don't give the table a name — just do it.

Always give a short reason after the value rather than the bare word:

```
**Verified:** ran-it — default workflow on the book for ~8 months
```

**Labs may only be built from `ran-it` cards.** Teaching an unrun step is how a curriculum ends up confidently wrong in front of a room. If a lab needs something marked `docs` or `inferred`, go run it and upgrade the card.

Flag uncertainty inline too: `[confirm-this: does this apply on the Team plan?]`

## Types

| Type | Use for |
|---|---|
| `concept` | An idea or mental model — context windows, evaluation, cost structure |
| `product-behavior` | How a product actually behaves, including surprises |
| `demo` | A small thing that can be shown live |
| `gotcha` | Something that bit you; the fix; how to avoid it |
| `pattern` | A reusable way of working that held up across cases |
| `integration` | Connecting Claude to another system — MCP, API, Salesforce |
| `preference` | Dave's working style or opinion — a "best practice," owned as his |
| `decision` | A settled project stance; nothing to verify |
| `question` | An open thread worth chasing |

`pattern` vs `preference`: a pattern is a technique that demonstrably works and that most people would benefit from. A preference is how *Dave* likes to work, where someone else could reasonably do the opposite. When in doubt, call it a preference — overclaiming is the more expensive error.

## One idea per card — split rather than stuff

**When two ideas could plausibly share a card, file two.** Narrow beats broad, and the tie-breaker is retrieval: a stuffed card matches many searches vaguely and none precisely, so it surfaces when it shouldn't and gets skimmed when it should have been read.

**The test: a card is a citable unit.** Size it to what you'd want a pointer to resolve to, not to a topic boundary. If you'd ever link to one half without the other, they're two cards. If they always travel together — same lesson, same "why it matters" — they're one. Card 006 keeps dictation errors and the glossary fix together for exactly this reason: you'd never cite the problem without the mitigation.

Split when the candidates differ in any of:

- **Who's searching for it** — different questions should reach different cards
- **Type** — a `gotcha` and a `preference` don't belong in one head matter
- **Section** — material for §5 and §3 serves different readers
- **How they resolve** — one may be settled while the other stays open

Cross-link the halves (`see card NNN`) so the connecting idea isn't lost. Related cards that point at each other are strictly better than one card that contains both.

## Naming

`NNN-short-description.md` — sequential as encountered. The number is for uniqueness; the description is for scanning. **Do not renumber to group by topic** — cross-references would break, and grouping is what sections are for.
