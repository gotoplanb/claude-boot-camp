# Lab Design — a Release-Notes Plugin With Tier *Commands*, Because Claude Can't Infer Which Release This Is

**Source:** Dave, Claude Boot Camp session 2026-09-17, wanting a plugin-authoring lab and landing on release tooling as the domain. The command-vs-skill ruling and the separate-files ruling are Beta's, same session.
**Type:** decision
**Verified:** `n/a` for the design. **Not yet a lab** — see the gate. Filed so the design survives until a plugin has actually been built once.
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

## Ruling 1 — tiers are *commands*, not auto-fired skills

This is the design point the lab exists to teach.

Skills fire when Claude **infers** relevance from context. But *"is this a patch or a major release"* is not inferable — it's something **you already know** the moment you're cutting the release. Leaning on automatic skill selection to guess correctly is asking the model to reconstruct a fact you could simply state.

That's card 014's failure mode: the model reasoning from its own read of the situation rather than being told. It's also exactly card 069's split — **the model decides a skill is relevant; you decide a command runs** — and release tier lands unambiguously on your side of that line.

So the lab isn't just "build a plugin." It's a worked example of choosing the right trigger, which is the judgment call that makes plugins good or annoying.

## Ruling 2 — three skill files, not one skill with conditionals

Keep them separate. One skill branching internally on tier would mean **a patch release pays the context cost of the major-release content** every time.

Three files is better progressive disclosure (card 059) and simpler to read. The instinct to unify them is code-reuse instinct applied where it doesn't pay — skills aren't functions, and duplication across them costs nothing at runtime.

## Ruling 3 — screenshots are out of scope for the skill itself

See card 071. A skill is inert instructions; **taking** a screenshot needs a live tool the host provides. The skill should *remind you to do it* and handle everything that doesn't need a tool — notes, changelog formatting, version-bump conventions.

That keeps the plugin portable across surfaces instead of half-working wherever computer use isn't available.

## The gate — shorter than card 068's

`labs/README.md`: labs only from `ran-it` cards. Outstanding:

| Question | Currently |
|---|---|
| The plugin file structure (`plugin.json`, `commands/`, `skills/`) | Card 069 — `docs`, stated without a source link |
| Does a command reliably load its paired skill? | Unverified |
| Command namespacing (`/release:patch`) behaves as expected | Unverified |

Unlike card 068 — which waits on product behaviour nobody controls — **this gate is just "build one."** Authoring the plugin once resolves every row and upgrades card 069 at the same time. It's a bootstrapping gate, not a blocked one.

## Why this matters

Plugin authoring is a section-5 capability with no lab behind it, and this is the rare candidate where the domain content is free and the mechanics are the actual lesson.

It also teaches the taxonomy by *using* it: a participant who finishes has made a deliberate command-vs-skill choice and seen why one skill per tier beats one skill with branches.

See card 069 (the taxonomy this applies), card 071 (why screenshots are out), card 059 (the context cost behind ruling 2), card 014 (don't make the model infer what you can tell it), card 068 (the other pending lab design), and card 025 (why the client-specific alternative isn't ready).
