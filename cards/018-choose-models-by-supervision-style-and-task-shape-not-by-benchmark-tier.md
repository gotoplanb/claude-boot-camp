# Choose Models by Supervision Style and Task Shape, Not by Benchmark Tier

**Source:** Dave describing his own model split, Claude Boot Camp session 2026-09-11 (verbatim account). The two-axis framing is Beta's, same session.
**Type:** preference
**Verified:** ran-it — this is the split he actually runs day to day across chat, Claude Code, and API work.
**Relevant to:** 1 (foundational concepts), 3 (administering — model policy), general

## Content

The naive framework is a capability ladder: reach for the most capable model you can afford. That's not how the choice gets made in practice. Two axes do more work — **fast/broad ↔ slow/precise**, and **attended ↔ unattended**.

### Sonnet — the conversational partner

Almost always the chat model. Fast, and capable enough for broad questions out of its training set, plus finding real documentation and links on the web — the kind of thing an open-source maintainer would have written down. Directional correctness is the bar; implementation details can stay unknown.

The property that actually earns it the slot is subtler than speed:

> *"I'm not thinking about being deliberate. I can just talk and let my mind flow and ramble, and Sonnet is fast and capable enough to help me — 'oh, you said that, you probably meant this' — and then we go back and forth and I can course-correct."*

**Sonnet is chosen because it makes unfiltered speech cheap.** No pre-filtering, no composing a careful prompt; ramble, get a fast read-back, correct. A slower, more deliberate model would be *worse* at this, because the value is in the loop speed, not the depth of any single answer. (This compounds with dictation — see card 006. When voice-to-text mangles a proper noun, a fast repair turn costs almost nothing.)

### Opus — code generation and grounded work

Actual code generation, and questions about Dave's own custom documentation and patterns. Attended work: he's in the loop while it happens, and precision matters because the answer has to be right against *his* material, not against public knowledge.

### Fable — autonomous, narrow, and watched for a different reason

**Rarely used for writing software** — *"I find it just kind of goes into a hole."* Almost no feature development.

Reserved for two shapes:
- **Security reviews** — but see card 019; this collides with Fable's classifiers.
- **Heavy refactors that touch a lot of existing test coverage** — large, mechanical, well-bounded, with tests as the check.

The nuance worth preserving, because it's easy to flatten into "fire-and-forget": he *does* watch it.

> *"I like watching what's going on. I like seeing the tool calls, I like seeing the decision making, because it makes me think. It's giving me feedback about how the model thinks."*

The task is autonomous; the *watching* is not supervision, it's **learning**. Observing a more capable model's decision-making is its own payoff, separate from whether the output is correct. That's a distinct motivation from auditing, and it's why "unattended" undersells what's happening.

### Haiku — the least-used model, and the most underrated slot

Used least overall, and only via the **Anthropic API** for asynchronous, well-specified tasks with simple outputs. The deciding property is latency plus cost:

> *"It's so inexpensive and fast it nearly feels like a synchronous call in a web application."*

Concrete fits: grab two bits of data, consider them together, return a comparison; biography generation; text rewriting; taking one source document and cutting it into different sizes and formats for different publishing channels.

## Why this matters

**The more capable model is used *less*.** Not on cost grounds — because the deciding factor is task shape and supervision style, not capability. Fable is for a narrow band of autonomous, bounded work. Most work is attended, so most work is Opus.

A pure capability ranking predicts the opposite and misleads in both directions: it pushes expensive models onto fully-specified tasks where a small model is strictly better, and onto interactive chat where latency *is* the product. It also leaves people surprised when the top-tier model "goes into a hole" on feature development.

Asking **"how specified is this, am I watching, and is speed part of the requirement?"** answers it in one step — and survives model releases, since a new tier changes which name sits in which row, not the rows themselves. That durability matters given how fast disposition shifts between versions (card 015).
