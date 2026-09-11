# Chat + MCP Beats Salesforce Mobile Because Pre-Built Views Don't Scale to Ad-Hoc Questions

**Source:** Dave, Claude Boot Camp session 2026-09-11, on using Claude for iOS against Salesforce data instead of the mobile app. Limits below are Beta's, same session.
**Type:** preference
**Verified:** `inferred` — a proposed daily-driver workflow, not yet run at length. Underlying artifact/connector mechanics are `docs`-verified (cards 028–030). `[confirm-this: use it as the primary mobile Salesforce surface for a week and record where it actually beat the app and where it didn't.]`
**Relevant to:** 6 (Claude + Salesforce), 2 (operating Claude products)

## The reframe

Salesforce Mobile's limitation isn't polish — it's **structural**. The app is built on **pre-built views**: page layouts, compact layouts, mobile cards. Someone has to design each view in advance, which means the app can only answer questions somebody anticipated.

Ad-hoc questions don't work that way. *"Across these contacts, who do I actually need to talk to?"* has no layout, and never will, because there are infinitely many such questions and each would need its own.

**Chat plus MCP sidesteps the whole model: there is no layout to design, because the view is generated per question.** That's a durable structural advantage, not a novelty — the same reason ad-hoc artifacts beat custom-report sprawl (card 031). Pre-built anything loses to arbitrary questions.

**Salesforce has since productized this pattern.** "Salesforce in Claude" — 37 prebuilt sales skills running inside Claude's interface, on the AIforce harness — is the supported version of exactly this (card 034). The DIY route keeps the advantage that matters here: prebuilt skills are pre-built views again, excellent for anticipated cases and useless for the unanticipated ones.

It also plays to what the model is actually good at: joining across records, synthesising an account's full history, bucketing, and judgment calls that are cumbersome in SOQL and natural in conversation.

## Three limits to be clear-eyed about

**1. It cannot discern what isn't logged.** The analysis is only as good as the org's activity data. If reps log calls inconsistently, Claude has the same blind spot a human flipping through the same records would have. The flexibility is in the *analysis*, not in filling gaps — and "probabilistic discernment" reads like it might compensate for messy data. It doesn't. Bad CRM hygiene produces confident, well-reasoned, wrong answers.

**2. Latency is real.** Joining across contacts and pulling an account's full history means **multiple tool calls** — SOQL governor limits and pagination don't disappear because Claude is driving. On a spotty connection that's noticeably slower than a native app's cached view. Fine for *"let me sit with this for a minute"*; wrong for *"I'm walking into the meeting right now."*

**3. Non-determinism has a boundary.** See card 033 — this is the important one, and it's general enough to stand alone.

## Why this matters

It reframes a complaint as a category error. Salesforce Mobile isn't badly built; it's solving a pre-built-views problem, and ad-hoc questions are outside that problem's shape. Knowing which shape you're in tells you which tool to reach for without relitigating it each time.

For consultants, it's also a credible answer to a complaint clients actually have — and one that needs no org configuration, which is the easiest kind of value to demonstrate.
