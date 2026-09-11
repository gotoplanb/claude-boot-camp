# Lab: Build a Knowledge MCP, Reach It From Your Phone, Then Swap the Source

**Status:** `draft` — designed, not yet run end to end.
**Section:** 4 (Integrating with Claude — MCP and APIs). Steps 2 uses section 5 material.
**Design source:** Dave, Claude Boot Camp session 2026-09-11.

> ⚠️ **Gate before teaching.** Steps 1–2 rest on `ran-it` cards (038, 040) and on a live example — davestanton.com/mcp serves markdown cards with frontmatter over OAuth. **Step 3 (Google Drive) has not been run.** Per `cards/README.md`, a lab may only be built from `ran-it` cards — run step 3 and card it before this is taught.

## What this proves

That you can put your own knowledge behind an MCP server and reach it securely from anywhere — **before** spending a quarter wiring Claude into Confluence.

It deliberately front-loads the question most enterprise AI projects answer last:

> **Will anyone actually use this, and which documents genuinely need to be in every conversation versus behind a pointer?**

## Prerequisites

- Claude Code, signed in
- Python (or your language of choice) for the server
- A handful of markdown files you actually care about
- A tunnel tool (ngrok or equivalent) for step 2
- For step 3: a Google account and the `gcloud` CLI or Drive API credentials

---

## Step 1 — A local MCP over your own markdown

Build the smallest possible MCP server that reads a directory of markdown files with frontmatter and exposes them as tools: `list_*` and `read_*`, plus a substring `search`.

**Why markdown with frontmatter:** the head matter carries the metadata that makes retrieval useful — type, status, tags, dates. This is not a toy scenario. It's the same shape as the corpus this curriculum is written in, and as Dave's book. **You are dogfooding a working pattern, not simulating one.**

**Expected output:** `list_*` returns your files with their metadata; `read_*` returns one; `search` finds a distinctive string.

**Watch for:** the tool *descriptions* are the product surface. A vague description means Claude never calls the tool (card 011).

---

## Step 2 — Reach it from your phone without exposing anything

Keep the server and Claude Code on your laptop. Expose only a control channel through a tunnel, and drive the session from the Claude iOS app.

**What this proves:** secure remote access with **nothing real on the public internet** — the tunnel carries UI, not credentials (card 040). This is the property most people assume requires a hosted enterprise deployment.

**Expected output:** you ask a question from your phone; the laptop's MCP server answers from your markdown.

**Watch for:** this works *because* compute stays local. Move the compute to someone else's infrastructure and your local server becomes unreachable (card 038).

---

## Step 3 — Swap the source for Google Drive `[not yet run]`

Replace "read markdown from the repo" with "fetch from Google Drive via a CLI call." Same tools, same shapes, different backing store.

**What this proves:** the pattern generalises to a real, messier data source — one with auth, pagination, and formats you don't control — **without** touching enterprise SSO complexity yet. Drive is a small, single, well-documented API surface.

**Expected output:** the same tool calls now return Drive content.

**Watch for:** how little of the MCP layer changed. The tool *shape* is stable; only the fetch changed.

---

## Stop here — deliberately

Do **not** build the Confluence / enterprise-knowledge-base connector as part of this lab. That's a different and much larger problem — governance, permission-per-document mapping, staleness — and it should not gate whether the core idea works.

This is crawl / walk / run, and the point of crawling is finding out whether the walk is worth taking.

### Take-home

Set the whole thing up yourself, end to end, on knowledge you actually use.

### Discussion — the honest version of "stop here"

Rather than pretending the run phase has no teeth:

1. What is the **smallest next step** from Google Drive toward *your* real enterprise source?
2. What **new problem class** does that introduce? (Mostly: permission-per-document mapping — see cards 042 and 043, where this becomes an identity problem rather than a plumbing one.)
3. What **stays the same**? (Mostly: the MCP tool-call shape. That stability is the reusable asset.)
4. Having built it — **which of your documents actually need to be in every conversation**, and which are fine behind a pointer? (Card 016.)

Question 4 is the one worth the hour. It's cheap to answer after this lab and expensive to answer after an enterprise integration.
