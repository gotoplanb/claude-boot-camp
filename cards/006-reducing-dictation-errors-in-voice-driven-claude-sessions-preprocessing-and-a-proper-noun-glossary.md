# Reducing Dictation Errors in Voice-Driven Claude Sessions — Preprocessing Apps and a Proper-Noun Glossary in Context

**Source:** Claude Boot Camp session, 2026-09-11, after "Claude for iOS" transcribed as "Quadfly OS" and "Claude Code" as "quiet code" in the same message.
**Type:** gotcha
**Verified:** ran-it for the failure mode (observed repeatedly in live sessions); `docs` for the mitigations. `[confirm-this: does SuperWhisper's custom vocabulary actually fix the Claude/Quadfly class of error, or does it need per-term training?]`
**Relevant to:** 2 (operating Claude products), general

## Content

A large share of this work is dictated — walking, driving, away from a keyboard. Speech-to-text is good at English prose and **bad at the proper nouns this domain is made of**. Real errors observed in one session:

| Dictated | Transcribed as |
|---|---|
| Claude for iOS | "Quadfly OS", "Claude Freyos" |
| Claude Code | "quiet code" |
| CLAUDE.md | "Claude MD", "Claude dot markdown" |
| framing.md | "framing dot markdown" |
| Sonnet | "sonic" |

The failure is systematic, not random: it hits exactly the words that carry the most meaning — product names, file names, model names — while the surrounding sentence transcribes fine. That makes it *more* dangerous than noisy transcription would be, because the sentence still reads as coherent.

### Mitigations, cheapest first

1. **Let the model infer from context.** Usually sufficient. "Quadfly OS chat that I called Beta" is unambiguous if the model knows the Alpha/Beta split. Requires no tooling — just enough project context loaded.
2. **Keep a proper-noun glossary in the model's context.** A short mapping of the terms this project actually uses, plus their common mistranscriptions, so the model corrects silently instead of guessing. Cheapest durable fix; lives in `CLAUDE.md` so it's auto-loaded.
3. **Preprocess before the model sees it.** A dictation front-end like [SuperWhisper](https://superwhisper.com) with a custom vocabulary, so "Claude" is never transcribed as "quiet" in the first place. Fixes it at the source rather than repairing downstream.
4. **Say it differently.** Use NATO-alphabet names for things you'll say constantly — which is exactly why the book's sessions are *Alpha*, *Beta*, and *Charlie* rather than names that transcribe unreliably.

## Why this matters

Two reasons, one practical and one that belongs in the curriculum.

Practically: an unrecognized mistranscribed proper noun sends a session down the wrong path, and because the sentence around it is fluent, neither party notices immediately. The cost isn't the typo, it's the wasted work downstream.

For the curriculum: voice is a first-class way of operating these products, and nobody's documentation covers the ergonomics of it. Designing *around* a transcription weakness — by naming things to survive dictation, or by loading a glossary — is a real technique for operating Claude well, and it generalizes to any voice-driven workflow.
