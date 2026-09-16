# Gaps — What the Lectures Still Need

Written after the first drafting pass over 56 cards (2026-09-12). Every section now has a lecture draft in [`sections/`](sections/). This is the list of what those drafts **can't** yet say, ranked by leverage.

The honest summary: **the corpus is strong on principles and thin on execution.** It can explain why a decision matters better than it can show someone doing it. That shows up as the same shape in §3, §4, and §6 — good reasoning, no worked example.

---

## Cross-cutting findings

These matter more than any single gap.

**1. One artifact unblocks two sections.** §6's keystone (a real Salesforce MCP connector) and §4's biggest hole (the remote-MCP deployment story) are the same project seen from two angles. Build one properly hosted MCP server against a real Salesforce org and you close the top gap in both sections at once. Nothing else in the list has that reach.

**2. Four sections independently want a decision tree.** §1 (which model), §2 (which surface), §3 (Claude Tag vs. DIY MCP vs. Cowork), §4 (local vs. remote). None exist. These are cheap — they're synthesis of cards you already have, not new research — and they're the highest value-per-hour items here.

**3. Cost is a hole in four sections and partly blocked externally.** Cards 034, 035, 036, and 048 circle cost without producing a number anyone can take to a CFO. Some of it is genuinely blocked on Salesforce publishing pricing. Some of it is just unmeasured.

**4. Short-shelf-life claims need an expiry convention.** Cards 027, 028, and 034 are product claims that will go false *silently* — nothing errors when a feature ships. Card 027 is a **negative** claim ("there's no path from artifacts into Projects"), which is the shortest-lived kind there is. Recommend adding a `Checked:` date line to product-behavior cards, in the spirit of card 056.

**5. The governance material has no lab at all.** Every card in §3's core (034, 042, 044, 045, 046, 047) is `docs` or `inferred`. Under the project's own rule, none of it can be labbed. §3 currently teaches recognition, not execution.

---

## The open questions are fewer than they look

32 `confirm-this` flags across the cards plus 5 open issues reads as 37 threads. Grouped by *what would actually resolve them*, it's about **six** (card 057):

| Blocker | Resolves | Effort |
|---|---|---|
| Team/Enterprise tenant — run Claude Tag + Cowork, build a plugin marketplace | cards 037, 046, 060; issues #6, #7; part of 007 | an afternoon |
| Publish artifacts, incl. connector-backed, with a second viewer | cards 027, 028, 029, 030 | an afternoon |
| One hosted Salesforce MCP connector | cards 030, 031, 032 | the keystone, days |
| One weeks-long session through real compactions | cards 008, 017 | elapsed time, not effort |
| Small standalone trials | cards 003, 006, 012, 016, 020, 059, 062, 064 | hours each |
| **Not actionable — waiting on someone else** | cards 002, 013, 034, 035, 036, 055, 061 | watch items, not work |

That last row matters: six flags are waiting on Salesforce pricing, a lawyer, or an undiagnosed event. They inflate the backlog without being work.

---

## Ranked: what to go do

### 1. Build and host a real Salesforce MCP connector — **the keystone**

Query Accounts and Opportunities, render results in a connector-backed artifact, run it against a real org, and host it so a hosted surface can reach it.

**Unlocks:** cards 030, 031, 032 move `inferred` → `ran-it`. §6's topic 6.2 gets its only possible lab. §4 gets the deployment story it's missing. The pattern cards get a reference implementation.

**Blocks without it:** §6 can lecture its headline claim and cannot demonstrate it. That's the single weakest point in the curriculum.

### 2. Deploy one MCP server properly and card the whole path

Hosting, auth, uptime, observability — what card 038 calls *"the forgotten second job."* Likely the same work item as #1.

**Why it matters:** this is the wall between a proof of concept and a system, and it's the first thing a CoE lead asks. The corpus currently stops at "works on my laptop."

### 3. Run Cowork or Claude Tag once, for real

Even a small setup.

**Unlocks:** §3's entire hands-on half, plus resolves open questions in cards 037, 045, and 046 — including whether a custom/enterprise MCP connector mirrors permissions or can carry its own credential, which changes the risk profile either way.

### 4. Write the four decision trees

Cheap, synthesis-only, immediately usable as handouts. Which model (§1), which surface (§2), which integration architecture (§3), local vs. remote (§4).

### 5. Run a weeks-long session with real compactions

**Unlocks:** card 017 `docs` → `ran-it`. Hooks are the structural answer to §5's central problem — state divergence — and right now the most important fix in §5.2 is documentation rather than experience.

### 6. Run the notes-and-diff elicitation protocol end to end

Boot camp issue #5. Upgrades card 025's protocol and generates new gotcha cards as a by-product.

### 7. Curate the §1 pointer list

Per the decision that foundations should link to Anthropic Academy and the docs rather than duplicate them: pick the actual pages, record a checked-on date. Boot camp issue #1. Half of §1's hour doesn't exist until this is done.

### 8. A week of deliberate mobile-first use, carded

§2's largest gap. Mobile is where a lot of real operating happens — including yours — and the corpus touches it only in passing (032, 052).

---

## Gaps that need thinking, not just doing

These won't be closed by running something.

**Evaluation.** The corpus knows how to verify a *task*. It has nothing on measuring whether a deployment delivers value against its cost — which is the question every CoE lead arrives with. The interesting version is organisation-specific, so it may not be linkable either.

**Week-one rollout.** A 5,000-person company says "we want Claude." What does the CoE lead stand up *before* usage arrives? All the governance cards assume the team and the problem already exist.

**Security as a topic.** Identity and access are well covered. Whether Claude introduces *new* attack surface — prompt injection against write-capable tools, exfiltration through output — is never asked. Card 044's "ordinary architecture, just faster" gestures at it without answering.

**Unattended Claude Code.** No card touches CI, GitHub Actions, or server-side agents. Every verification pattern assumes a human nearby.

**When not to use Claude.** The corpus is intensely about using it well. Card 033 is the closest thing to a scope boundary and it's Salesforce-flavoured.

**Capability control as policy.** Card 018 is a personal model-selection heuristic. Turning it into something a hundred people follow is uncarded — and without it, §3's cost argument doesn't hold.

---

## Section readiness

| Section | Lecture | Labs | Blocking gap |
|---|---|---|---|
| 1 · Foundational | Thin by design — pointers + short original spine | Weak | Pointer list uncurated |
| 2 · Operating | **Two solid hours** | Good for 2.1, unrun for 2.2 | Mobile; connector how-to |
| 3 · Administering | Coherent as a *lens*, not a domain | **None possible** | Nothing runnable in the governance cluster |
| 4 · Integrating | Two hours, honest about what's unbuilt | Decent | Deployment story; token exchange unbuilt |
| 5 · Building | **Three hours — strongest** | **Strongest** | Hooks unrun; nothing on unattended use |
| 6 · Salesforce | Topic 6.1 strong; 6.2 undemonstrable | 6.1 only | The keystone connector |

**Teach-tomorrow order:** §5, then §2, then §6.1. Those rest on `ran-it` material throughout.
