# Optimizing Against Representative Data Discards Hedges You Didn't Know You Had

**Source:** Beta, Claude Boot Camp session 2026-09-26, pushing on the eval design in card 076's sorting example.
**Type:** gotcha
**Verified:** `inferred` — a predicted trap, **not observed**. The library-internals facts below are standard reference knowledge, not checked against a source in this session. *[confirm-this: actually run it — have a model design a specialized sort for a representative dataset, then evaluate it against adversarial and edge-case inputs as well as a distribution holdout, and record what breaks.]*
**Relevant to:** 1 (foundational concepts), 5 (building with Claude Code), general

## The trap

Card 076's method ends with an eval: run half a holdout through the specialized implementation, half through the general one, compare performance. If it wins, ship it.

**That eval measures the wrong thing**, and the reason is instructive well beyond sorting.

A general-purpose library sort is not a naive choice that nobody bothered to optimize. Timsort, introsort and friends are **hybrids on purpose** — they are hedging against cases that don't appear in typical data:

- nearly-sorted input
- reverse-sorted input
- large numbers of duplicates
- adversarial input that tips a naive algorithm into its O(n²) case

Those hedges cost a little on average. That average cost is **exactly what a specialized implementation wins back** — which is why it looks so good on a representative holdout.

You didn't beat the library. You **removed its insurance** and measured the premium.

## The guarantee nobody tests for

Worse than a performance cliff is a dropped property. **Stability** — whether equal elements keep their relative order — is the classic one: easy to lose, easy not to notice you've lost, and it breaks things far downstream of the sort. Nothing errors. A secondary ordering someone relied on two modules away just quietly stops holding.

The general shape: **an optimizer optimizes what you measured, and silently discards what you didn't.**

## What the eval actually needs

- **Adversarial and edge-case inputs deliberately included** — not more of the same distribution split in half. A holdout drawn from representative data cannot surface a worst case that representative data doesn't contain.
- **Named guarantees checked explicitly.** Stability, in-place-ness, memory ceiling, worst-case bound. Write them down *before* the model proposes anything, or you won't notice one going missing.
- **Distribution drift treated as a when, not an if.** "Optimized for our data" means "coupled to today's data." The win expires the day the data changes, and nothing announces it.

Only then does *"is the juice worth the squeeze"* mean anything.

## Why this matters

It's the honest counterweight to card 076. That card says use the model at build time to produce something specialized — and specialization is exactly the move that trades robustness for average-case speed. That trade can be right. It just has to be made **knowingly**, with the thing you're giving up written down.

Generalizes past sorting to every "we replaced the generic thing with something tuned for us": **the generic thing's inefficiency is often a hedge, and the benchmark that makes your replacement look good is usually the one that omits the case the hedge was for.**

And it puts a real obligation on the specialized path. A library sort is maintained, adversarially tested, and used by millions. Your generated implementation has a test suite you wrote in an afternoon, against inputs you thought of.

See card 076 (the method this bounds), card 064 (the same shape in a different domain — a harvested sequence that looks equivalent and isn't), card 020 (a verifier standing in for you, and the care its design needs), and card 021 (verification checks ground truth after — but only the ground truth you thought to check).
