# Cards

The raw material of the curriculum. One idea per file, filed **as encountered** — not organized by section. Organization happens later, during synthesis.

Expect 100+ of these before a section gets seriously written.

## File format

```markdown
# [Short descriptive title]

**Source:** [where this came from — docs URL, a real session, a failure you hit, a conversation]
**Type:** concept | product-behavior | demo | gotcha | pattern | integration | question
**Verified:** ran-it | docs | inferred | confirm-this
**Relevant to:** [section number(s) or "general"]

## Content

[The thing itself. Commands, output, the behavior, the idea.]

## Why this matters

[Why it earns a place in the curriculum — what it lets someone do, or what it saves them from.]
```

## The Verified field

Much of this curriculum covers products that hadn't been used day-to-day when the card was written. The `Verified:` field keeps that honest.

| Value | Meaning |
|---|---|
| `ran-it` | Actually executed and observed. Highest confidence. |
| `docs` | Read in official documentation, not yet run. |
| `inferred` | Reasoned from related behavior. Lowest confidence. |
| `confirm-this` | An open question to resolve. |

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
| `question` | An open thread worth chasing |

## Naming

`NNN-short-description.md` — sequential as encountered. The number is for uniqueness; the description is for scanning. **Do not renumber to group by topic** — cross-references would break, and grouping is what sections are for.
