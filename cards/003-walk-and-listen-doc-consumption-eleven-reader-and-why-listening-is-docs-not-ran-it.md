# Walk-and-Listen Documentation Consumption — Eleven Reader, a Local TTS Fallback, and Why Listening Counts as `docs` Not `ran-it`

**Source:** Claude Boot Camp planning session, 2026-09-11, discussing how Dave actually prefers to consume documentation.
**Type:** preference
**Verified:** `ran-it` for Eleven Reader itself — in regular use, and confirmed 2026-09-12 reading a full technical post end to end (14 minutes of audio, headings and structure preserved, word-level highlighting). `docs` for the batch-automation options below, which remain untried. `[confirm-this: how the audio handles **code blocks** specifically is still unknown — the post tested had prose and tables but no code.]`
**Relevant to:** general (affects how learning happens across every section)

## Content

Reading docs at a desk is not the preferred mode — walking and listening is. The lightweight path:

- **[Eleven Reader](https://elevenreader.io)** (ElevenLabs app) — import a URL or paste document text, it generates audio, listen on the go. No pipeline, no automation, works for casual use today. davestanton.com now carries a **Listen** button that hands a post straight to it via the share sheet — see card 052 for why that's a share-sheet trick rather than a deep link.

If batch or queued listening ever becomes worth automating:

- Scrape doc pages to markdown, hit the **ElevenLabs API** per page, concatenate into one file; or
- Reuse the existing **local Piper TTS endpoint** (`POST /tts`) from the `conduct` repo — free and fully local, at lower voice quality.

Don't build the pipeline until the manual version proves annoying enough. The app covers the actual use case now.

## The verification consequence

**Listening to documentation is `docs`, not `ran-it`.** Audio is an *input channel*, not a form of verification. A card whose knowledge came from hearing a doc read aloud carries exactly the same confidence as one from reading that doc on a screen — which is to say, plausible but unconfirmed in practice.

This is worth stating explicitly because audio *feels* more like experience than reading does. You were moving, it took real time, you engaged with it. None of that is execution. If it wasn't run, it isn't `ran-it`, and it can't back a lab.

## Why this matters

The walk-and-listen loop is how a large share of this curriculum's raw input will arrive, so it needs a clear verification rule attached or confidence inflates quietly. Getting that rule pinned down before the volume ramps up is cheaper than auditing 80 cards later to work out which ones were only ever heard.
