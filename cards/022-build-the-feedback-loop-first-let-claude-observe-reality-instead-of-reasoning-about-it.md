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

## Which browser tool — and the correction worth knowing

Verified 2026-09-11 against [Use Claude Code with Chrome](https://code.claude.com/docs/en/chrome), [Computer use](https://platform.claude.com/docs/en/build-with-claude/computer-use.md), and [Playwright MCP](https://playwright.dev/docs/getting-started-mcp).

**Claude in Chrome is not a separate standalone agent.** It's a [browser extension](https://chromewebstore.google.com/detail/claude/fcoeoabgfenejglbffodgkkbkcdhcgfn) that Claude Code drives over native messaging and the Chrome DevTools Protocol — you invoke it from the same session (`--chrome` / `/chrome`) and the docs pitch it precisely as testing and debugging "without switching contexts." It's context-isolated only in the sense that any tool is: it doesn't share Claude's reasoning, it's something the session *uses*, like Bash.

The distinction that actually matters for "see it with your own eyeballs":

| | **Claude in Chrome** | **Playwright MCP** |
|---|---|---|
| First-party? | Yes, Anthropic | No — [Microsoft maintains it](https://github.com/microsoft/playwright-mcp) |
| How Claude perceives the page | **Visually** — screenshots | Primarily the **accessibility tree / DOM** |
| Good for | Design review, UI debugging, "does this look right" | Form automation, deterministic structural assertions |
| Needs | Extension installed, Chrome running, `/login` auth (not API key) | A browser instance you provide |

So for Dave's stated goal — *see it with your own computer eyeballs* — **Claude in Chrome is the closer fit**, not Playwright. That inverts the intuition that the MCP route is automatically better because it's "in the session." Both are in the session; they differ in what the model perceives.

The other first-party option is [Computer Use](https://platform.claude.com/docs/en/build-with-claude/computer-use.md) — vision-based, works on native apps too, less mature for browser work.

*(Session note, `ran-it`: the Docker MCP Playwright server registered but exposed **zero tools** on 2026-09-11; driving local Chrome via Playwright from the shell worked, and screenshots read back fine as images — so "Playwright can't be visual" is too strong a generalisation, it's a statement about the MCP server's default snapshot mode. Verify whichever browser path you pick actually loads before depending on it.)*

## Why this matters

Dave calls this the most important tooling decision he's made, and the reason is leverage rather than correctness alone: every hour spent on instruments pays out on every subsequent task, whereas an hour spent correcting one hallucinated build claim pays out once.

It also changes what "done" can mean. Without a loop, "done" is the model's assertion and your review is the only check. With one, "done" is a state the model can verify before it ever reaches you — which is the precondition for walking away at all (card 020).
