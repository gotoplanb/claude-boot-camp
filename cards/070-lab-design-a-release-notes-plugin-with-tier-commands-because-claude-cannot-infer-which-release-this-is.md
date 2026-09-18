# Lab Design — a Release-Notes Plugin With Tier *Commands*, Because Claude Can't Infer Which Release This Is

**Source:** Dave, Claude Boot Camp session 2026-09-17, wanting a plugin-authoring lab and landing on release tooling as the domain. The command-vs-skill ruling and the separate-files ruling are Beta's, same session.
**Type:** decision
**Verified:** `ran-it` — **built, published and installed**, 2026-09-17. Lives at [gotoplanb/claude-plugins](https://github.com/gotoplanb/claude-plugins) under `plugins/release`; first built in a `watchtower/labs/` directory that has since been moved out. Authored, invoked headlessly against real tag ranges, then distributed through a marketplace and reinstalled from it. Confirmed 2026-09-18: the three release skills appear in Claude for Mac under Customize → Skills → Yours → **"From marketplaces you added"**, tagged with the plugin's displayName and attributed to their author. *[confirm-this: whether the skills also **execute** correctly on that surface is a separate question — see card 071.]* The build resolved the gate and corrected Ruling 1's mechanism (below).
**Relevant to:** general (curriculum structure), 5 (building with Claude Code), 2 (operating Claude products)

## Why this domain and not the client one

The content already exists. Semver, changelog formats, what a good release note actually says — these are **writable today**, not tacit knowledge that has to be excavated first.

That's the difference between this and a Bosshardt-style lab, which is currently a *"figure out what I even know"* problem (card 025's knowledge-elicitation shape) before it can be a build problem. Release conventions are also generic enough to be relatable to nearly any participant, which a client-specific build isn't.

## The shape

Three skills — patch, minor, major — each triggered by a matching command:

`/release:patch` · `/release:minor` · `/release:major`

Each scaled to what that tier actually needs:

- **patch** — a tight changelog-entry format, not much more
- **minor** — the middle case
- **major** — the fuller treatment: migration notes, breaking-change callouts, and a reminder to grab screenshots yourself

## Ruling 1 — the model must not guess the tier

This is the design point the lab exists to teach. **The judgment below held; the mechanism I first proposed did not — see "What building it changed."**

Skills fire when Claude **infers** relevance from context. But *"is this a patch or a major release"* is not inferable — it's something **you already know** the moment you're cutting the release. Leaning on automatic skill selection to guess correctly is asking the model to reconstruct a fact you could simply state.

That's card 014's failure mode: the model reasoning from its own read of the situation rather than being told. Card 069 frames the same split as a question — **should the model be allowed to fire this on its own?** — and release tier lands unambiguously on *no*.

So the lab isn't just "build a plugin." It's a worked example of choosing the right trigger, which is the judgment call that makes plugins good or annoying.

## What building it changed

The design treated **command** and **skill** as two primitives with different trigger semantics. They aren't — commands have been merged into skills (card 069). The choice is a frontmatter flag:

```yaml
disable-model-invocation: true
```

So all three tiers are **skills carrying that flag**: invocable as `/release:patch`, never model-inferred. There is deliberately **no `commands/` directory** — paired command files would collide on the same `/release:*` names.

This makes the lab *better*, not weaker. "Should the model be allowed to fire this?" now maps to one concrete, teachable knob instead of an architectural decision.

## Ruling 2 — three skill files, not one skill with conditionals

Keep them separate. One skill branching internally on tier would mean **a patch release pays the context cost of the major-release content** every time.

Three files is better progressive disclosure (card 059) and simpler to read. The instinct to unify them is code-reuse instinct applied where it doesn't pay — skills aren't functions, and duplication across them costs nothing at runtime.

**The rule generalizes past tiers.** A skill that *starts* a process and a skill that *is* the pipeline — many calls in sequence, then a synthesis — are different in kind, not merely different steps. Chained skills are pipe-and-filter: keep the orchestration trigger and the pipeline logic in separate files for the same reason patch and major are separate.

## Ruling 3 — screenshots are out of scope for the skill itself

See card 071. A skill can run the tools it declares in `allowed-tools` — these three declare git plus Read/Edit/Write — but it cannot conjure a capability the host doesn't have, and screen capture is one. So the built skill leaves `![TODO: screenshot — <what to shoot>]()` placeholders at the spots needing an image and lists them back as a checklist, while handling everything that needs no live tool: notes, migration steps, changelog formatting, version math.

That keeps the plugin portable across surfaces instead of half-working wherever computer use isn't available.

## The gate — resolved by building it

`labs/README.md`: labs only from `ran-it` cards. All three rows cleared on 2026-09-17:

| Question | Outcome |
|---|---|
| Plugin file structure | ✅ Manifest is `.claude-plugin/plugin.json`, `skills/` at plugin root. Card 069 corrected. |
| Does a command reliably load its paired skill? | ✅ **Question dissolves** — same primitive, so there's no command→skill hop to be unreliable. `/release:patch` *is* `skills/patch/SKILL.md`. |
| Namespacing behaves as expected | ✅ Namespace = plugin `name` + skill `name`. Ran headless and produced correct tier output. |

Exactly as predicted, this was a **bootstrapping gate, not a blocked one** — unlike card 068, which still waits on product behaviour nobody here controls.

## Why this matters

Plugin authoring is a section-5 capability with no lab behind it, and this is the rare candidate where the domain content is free and the mechanics are the actual lesson.

It also teaches the taxonomy by *using* it: a participant who finishes has made a deliberate command-vs-skill choice and seen why one skill per tier beats one skill with branches.

See card 069 (the taxonomy — corrected by this build), card 071 (why screenshots are out, and `allowed-tools` as the alternative), card 059 (the context cost behind ruling 2), card 014 (don't make the model infer what you can tell it), card 068 (the other lab design — still gated), card 025 (why the client-specific alternative isn't ready), and card 072 (judgment vs. mechanism).
