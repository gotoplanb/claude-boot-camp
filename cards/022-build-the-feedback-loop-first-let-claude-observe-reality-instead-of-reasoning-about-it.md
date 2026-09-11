# Build the Feedback Loop First — Let Claude Observe Reality Instead of Reasoning About What Should Have Happened

**Source:** Dave, Claude Boot Camp session 2026-09-11: *"providing that feedback loop has been probably the most important tooling decision and focus that I've done."* Connection to the state-divergence problem is Beta's, same session.
**Type:** preference
**Verified:** `ran-it` — this is Dave's standing build practice, implemented in his open-source [Watchtower](https://github.com/gotoplanb) local Grafana stack and used across projects.
**Relevant to:** 5 (building with Claude Code), 4 (integrating — MCP), 1 (foundational concepts)

## The principle

> **Don't let the model reason about what should have happened when it could look at what did.**

Or as Dave puts it to Claude: *"Don't tell me you built the thing. Build it right."*

A model left without instruments will hypothesize — infer from the code what the tests probably do, guess what an endpoint returns, assume a page renders. Each guess is plausible and independently checkable, and checking is the whole game.

Dave's observability day job biased him toward this, and the resulting tooling has two audiences worth distinguishing: instruments that let **him** see what happened, and — more importantly — instruments that let **Claude itself** see what it just did.

## The loop, concretely

1. **Unit tests first, ~90% coverage**, before moving on. The cheapest verifier, and the one that runs on every change.
2. **Drive the real browser.** Playwright (or the Docker MCP toolkit) from inside the session, so Claude loads the running app and *sees* it — rather than curling an endpoint and inferring from HTML what a user would experience.
3. **Read the traces.** A local Grafana stack (Watchtower) with Tempo, checked *after every step*. Claude doesn't hypothesize what happened between services; it opens the trace and reads what actually did.

Each rung replaces an inference with an observation. Together they cover the three ways a build lies: logic wrong (tests), presentation wrong (browser), behavior-between-components wrong (traces).

## Why this works where a `CLAUDE.md` reminder didn't

This is card 014's problem — the model trusting its own snapshot over re-querying the world — solved generally rather than argued with.

The reason it sticks is the same as card 016's: **it isn't standing vigilance, it's a cheap action bound to a trigger.** "After every step, look at Tempo" names a moment and an unambiguous move. "Always verify your assumptions" names neither and loses every turn.

The other half is cost. Making observation a single tool call away means it happens; if checking a trace required leaving the session, it wouldn't. **Watchtower is the git-log drift problem solved by making "go observe reality" as easy as a tool call, instead of something the model has to remember to do on its own initiative.**

## Claude in Chrome vs. a browser tool inside the session

Worth distinguishing rather than treating as equivalent. `[confirm-this: characterisation pending verification against official docs — do not treat as settled.]`

The property that matters for this loop is **whether the observation and the code-writing share one context**. Seeing the bug and writing the fix in the same breath is the point; an observation made in a separate agent has to be carried back by hand, which reintroduces exactly the gap this loop exists to close.

*(Session note: the Docker MCP Playwright server registered but exposed no tools in a live attempt on 2026-09-11 — driving a local Chrome via Playwright from the shell worked instead. Verify your browser path actually loads before depending on it.)*

## Why this matters

Dave calls this the most important tooling decision he's made, and the reason is leverage rather than correctness alone: every hour spent on instruments pays out on every subsequent task, whereas an hour spent correcting one hallucinated build claim pays out once.

It also changes what "done" can mean. Without a loop, "done" is the model's assertion and your review is the only check. With one, "done" is a state the model can verify before it ever reaches you — which is the precondition for walking away at all (card 020).
