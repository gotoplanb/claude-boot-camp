---
title: "5. Building with Claude Code"
order: 5
status: drafting
---

## Scope

From an empty repo to a deployed, tested artifact.

The best-covered section in the corpus — 15 `ran-it` cards. It supports **three hours**, and they build on each other in order.

---

## Topic 5.1 — Instructions lose; structure wins

*The conceptual hour. Everything else in the section is this principle applied.*

### The spine

**If you're writing longer, louder, or more insistent instructions to get reliable behaviour, the instruction is the wrong tool. Change the situation instead.** (Card 023, `ran-it`.)

### Lecture arc

**1. Open with the failure, not the principle (card 014, `ran-it`).**
You tell Claude, in `CLAUDE.md`, to always check git state before asserting it. It doesn't. Not because it's disobedient — because the model's own snapshot of the world is high-confidence ground truth *to itself*, and a standing instruction has to win that argument on every single turn. It usually does. It doesn't have to lose often to be unreliable.

The behaviour here is `ran-it` and repeated. The *mechanism* — the model trusting its own snapshot — is inferred, and worth saying so.

**2. Name the pattern (card 023).**
This is the umbrella the practical cards keep rediscovering independently. The fix is never a better-worded instruction. It's removing the need for the instruction: keeping work in-session, binding an action to a trigger, or putting a deterministic hook in the harness so compliance isn't a choice the model makes.

**3. Two kinds of harness (card 021).**
**Constraint** narrows the space *before* generation — Tailwind's closed vocabulary, a framework choice, a curated set of primitives. It encodes taste and prevents drift. **Verification** checks reality *after* — tests, a real browser, traces. They fail differently: constraint can't catch broken logic, verification can't prevent stylistic drift. Most real setups need both, and knowing which one you're reaching for is the skill.

The constraint half is `ran-it`; the two-category framing itself is inferred.

**4. Unattended work needs an external verifier (card 020).**
You can walk away from a task exactly when something *other than your own reading of the output* can tell whether it worked. Tests, builds, a diff against a golden file. If the deliverable is judgment, there is no external verifier and you will supervise regardless — which is fine, as long as you planned for it.

**5. Determinism buys debuggability, not correctness (card 033, `inferred`).**
Where output drives a decision someone will later question, prefer a rollup or a formula. It's usually *less* accurate than the model and still the right call, because it's wrong the same way every time — and that is what makes it defensible and fixable.

**6. Spend intelligence at build time (card 048, `ran-it`).**
A frontier model builds the thing; the thing doesn't call a model to run. The test: **if you can state the rule, encode it. If stating the rule is the hard part, that's where the model belongs.** Swapping five photos and a price into a PDF is PyMuPDF, not an LLM call — even though the LLM-per-request version demos better and bills four figures a month.

### Takeaway

Ask of any unreliable behaviour: *am I trying to instruct my way out of a structural problem?*

---

## Topic 5.2 — Context, state, and keeping the model in sync

### The spine

The model's picture of the world goes stale silently, and the fixes are structural.

### Lecture arc

**1. The passive/active axis, applied locally (card 011, `ran-it`).** Same axis as §2.1. `CLAUDE.md` is passive and always current because it's read from disk; MCP is active and fetched on demand.

**2. Keep `CLAUDE.md` small, and point (card 016, `ran-it`).** A minimal core plus triggered pointers — *"when cutting a release, read `docs/release-checklist.md` first"* — beats one large file. The detail loads when relevant instead of costing attention every turn. Carries `[confirm-this: how reliably does a Projects chat actually follow a pointer to a connector-backed doc?]` — the local behaviour is verified, the chat-surface behaviour is reasoned.

**3. Build the feedback loop first (card 022, `ran-it`).** Three rungs, each replacing an inference with an observation: unit tests, then drive a real browser so Claude sees rendered UX rather than HTML, then read traces so it isn't hypothesising about service calls. Don't let the model reason about what should have happened when it could look at what did.

**4. Compaction drops dynamic state (card 017, `docs`).** `CLAUDE.md` is re-injected from disk; git and world state are not. A `SessionStart` hook with the `compact` matcher can re-inject them. **Flagged honestly:** every mechanic here is from the hooks reference and carries `[confirm-this: needs a real weeks-long session with multiple compactions before this backs a lab.]`

### Takeaway

Assume the model's world-state is stale unless something structural refreshed it.

---

## Topic 5.3 — The two-session pattern, and building your own tools

### The spine

Exploration and filing are different disciplines and want different sessions — and building the tool is often how you think the problem through.

### Lecture arc

**1. Seminar chat and code session (card 001, `ran-it`).** Exploration wants to wander; filing wants precision. Running them as one session makes both worse. Months of use; the corpus itself is the output.

**2. Building the tool is the thinking (card 009, `ran-it`).**

**3. Small-team credential practice (card 041, `ran-it`).** Covered properly in §3/§4; here it's just enough to get a workspace running.

**4. Publishing from Claude Code (card 028, `docs`).** Requires `/login` auth rather than an API key, plus version and plan gates. Name the gate; don't teach the flow as experience until someone runs it.

### Takeaway

Separate the session that explores from the session that produces, and notice when writing the tool is the fastest way to understand the problem.

---

## Labs — provisional

Strongest candidates, all from `ran-it` cards. These are sketches, not the built article.

| Lab | Card | What they do |
|---|---|---|
| Feedback loop | 022 | Write tests, then have Claude drive a real browser and screenshot what rendered |
| Constraint harness | 021 | Build a page under Tailwind's vocabulary; compare drift against hand-rolled CSS |
| Minimal core + pointer | 016 | Five-line `CLAUDE.md` plus a triggered pointer; watch it load only on the trigger |
| Two-session handoff | 001 | Explore in chat, paste to Claude Code, file cards — observe cards on disk, not an essay |
| Build-time refactor | 048 | Convert an LLM-per-request job to build-time schema + deterministic run; compare cost |

**Not yet labbable:** the hooks material (017) is `docs` only. Building a lab on it would violate the project's own rule.

---

## What's verified and what isn't

| Claim | Card | Status |
|---|---|---|
| Instructions lose to structure | 023 | `ran-it` |
| State divergence / stale snapshot | 014 | `ran-it` behaviour, `inferred` mechanism |
| Build-time vs. run-time economics | 048 | `ran-it` — built and shipped |
| Constraint harness | 021 | `ran-it`; two-category framing `inferred` |
| Minimal core + triggered pointers | 016 | `ran-it` locally; `[confirm-this]` on Projects chat |
| Feedback loop rungs | 022 | `ran-it`; Chrome-vs-Playwright ranking is preference |
| External verifier enables unattended work | 020 | `ran-it` observation, `inferred` generalisation; `[confirm-this: run the Python-prototype → Fable-ports-to-Rust task...]` |
| Compaction hooks | 017 | `docs` only — **do not lab** |
| Publishing artifacts from Claude Code | 028 | `docs` only |
| Determinism buys debuggability | 033 | `inferred` |

---

## Gaps — where more hands-on time is needed

**1. Nothing covers Claude Code running where nobody is watching.** No card touches CI, GitHub Actions, or server-side agents. Every verification pattern in the section assumes a human with eyeballs nearby. For a CoE audience planning automated use, that's the largest hole.

**2. Hooks are documented but unrun.** Card 017 is the structural answer to the section's central problem and it's `docs`-only. One weeks-long session with real compactions converts the most important fix in §5.2 from theory to experience.

**3. No budget ceiling story.** Card 048 says where to spend intelligence; nothing says how to cap exposure in Claude Code, or what happens when you hit a limit.

**4. The remote-control tunnel is a black box.** Card 040 is `ran-it` for you and opaque to everyone else — no script, no repo, no worked setup.

**5. No attended/unattended checklist.** Card 020 states the principle but never proceduralises it into questions you answer *before* handing a task off.

**6. When *not* to use Claude is barely addressed.** The section is intensely about using it well. Card 033 is the closest thing to a scope boundary, and it's Salesforce-flavoured.
