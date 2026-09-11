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

## Naming

`NNN-short-description.md` — sequential as encountered. The number is for uniqueness; the description is for scanning. **Do not renumber to group by topic** — cross-references would break, and grouping is what sections are for.
