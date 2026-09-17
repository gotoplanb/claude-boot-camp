---
title: "2. Operating Claude Products"
order: 2
status: drafting
---

## Scope

Using the products well, day to day. Which surface for which job, how context actually gets into the window, and the habits that separate a productive session from a chaotic one.

The cards do not support one hour. They support **two**, and they divide cleanly.

---

## Topic 2.1 — Context, and which surface holds it

*One hour. Lecture ~20 min, lab ~30 min, takeaway ~10 min.*

### The spine

Every mechanism for giving Claude knowledge — a Project's knowledge base, a `CLAUDE.md`, an MCP server — sits on **one axis: passive context versus active retrieval.** Pick by mechanism, not by preference.

### Lecture arc

**1. The axis (card 011, `ran-it`).**
Passive context is present every turn. It costs attention, it can't be forgotten, and it goes stale silently. Active retrieval is fetched on demand: perfectly fresh, and skippable — the model has to *choose* to call it. The failure modes are opposites. Passive fails by dilution, active fails by omission. Most good setups are a hybrid: a small passive pointer that tells the model the active path exists.

**2. So Projects and CLAUDE.md aren't competitors (cards 007 `docs`, 008 mixed).**
They're the same idea on different surfaces. A Project is a knowledge base plus custom instructions scoped to a set of chats on claude.ai — right for stable reference material across exploratory work. `CLAUDE.md` is repo-scoped, versioned in git, and always current because it's read from disk. Both can exist for the same body of knowledge. The question is never "which is better," it's "which surface am I on."

**3. Relevance beats volume — and the old instinct was right for the wrong reason (cards 008, 011).**
Rationing the context window used to be about *size*. It isn't anymore. A large irrelevant knowledge base still degrades retrieval, so the habit survives — but the reason changed from "you'll run out of room" to "you'll bury the signal."

**4. Triggered pointers beat standing vigilance (cards 014, 016, both `ran-it`).**
This is the practical heart of the hour. A standing instruction — *"always re-check git state before asserting it"* — competes on every single turn against the model's confidence in its own snapshot, and loses often enough to matter. A **conditional pointer** — *"when doing X, read path Y"* — fires reliably, because it's bound to a trigger rather than to continuous attention.

**5. Which means the real fix is structural (card 017, `docs`).**
If the model has to choose to comply, you have a reliability problem. A hook runs deterministically through the harness whether or not the model decides it's relevant. Note the honest limit here: card 017 is `docs`-verified against the hooks reference and has not been run through a weeks-long session with multiple compactions.

### Lab (~30 min) — Projects vs. CLAUDE.md, head to head

From card 008 (`ran-it` for the CLAUDE.md half).

1. Put the same 3–5 reference documents in a claude.ai Project **and** in a repo's `CLAUDE.md`.
2. Ask both surfaces the same question. Compare the answers.
3. **Now edit one of the source documents in the repo.** Ask again.
4. Watch `CLAUDE.md` reflect the edit immediately and the Project keep answering from the stale upload.

The staleness is the whole lesson, and it lands much harder when they cause it themselves than when they're told about it.

### Takeaway

You don't choose between context mechanisms — you choose **where on the passive/active axis** a given piece of knowledge belongs, then pick the surface that implements it. And when you need reliable behaviour, bind it to a trigger instead of asking for vigilance.

---

## Topic 2.2 — Artifacts, and who they run as

*One hour. Lecture ~20 min, lab ~25 min, takeaway ~15 min.*

### The spine

An artifact is an **output tray, not a knowledge base** — and a connector-backed one executes as *the viewer*, not as you.

### Lecture arc

**1. Artifacts don't loop back (card 027, `docs`).**
There's no retrieval path from an artifact into a Project's knowledge base or a future chat. To reuse one you download it and upload it again. Artifacts are for sharing finished work or iterating inside a single session.

**2. Claude Code can publish them, behind an unexpected gate (card 028, `docs`).**
It requires `/login` auth to a claude.ai account — an API key won't do it — plus not being on ZDR/HIPAA/CMEK, not on Bedrock/Vertex, and Claude Code ≥2.1.183. The coupling of *authentication method* to *publishing capability* is the part nobody predicts.

**3. The permission model is inverted from the obvious one (card 029, `docs`).**
A connector-backed artifact runs **as whoever opens it.** Two people load the same dashboard and see different data, governed by their own accounts. Side effects — posting, updating — execute as the clicker. The natural mental model is "a shared view of what I saw." That's wrong, and it fails *silently*: no error, just different numbers.

**4. So pick the data mode deliberately (card 030, `docs`).**
Embedded snapshot: pulled under your permissions, point-in-time, and therefore **must** be shared org-scoped rather than public. Connector-backed: each viewer's own access governs, which preserves the permission model you were careful about — at the cost of every viewer needing the connector.

**5. Say which one it is when you share it.**
A link that looks like a dashboard gets treated like one. Title a point-in-time pull as a point-in-time pull, or someone quotes last week's numbers in a meeting.

### Lab (~25 min) — publish, then open it as someone else

Publish a connector-backed artifact from Claude Code, then have a second person open the link and compare what each of you sees.

**Honest gate:** card 029 is `docs`-verified, not `ran-it`. Running this lab is itself the verification — it resolves `[confirm-this: publish a connector-backed artifact and have a second person open it; confirm the per-viewer permission prompt and the differing data.]` Until someone does, treat the lab as an experiment rather than a demonstration.

### Takeaway

Before you share an artifact, answer two questions: **is the data embedded or live**, and **whose permissions will run it**. Everything that goes wrong with artifacts goes wrong at one of those two.

---

## Topic 2.3 — Extending a chat surface: connectors, skills and plugins

*One hour. Lecture ~20 min, lab ~25 min, takeaway ~15 min.*

### The spine

Everything you bolt onto a chat surface has a standing cost and, in one case, a credential. The decision is **narrower than people expect** — most accounts should connect less than they do.

### Lecture arc

**1. One account per provider (card 065, `ran-it`).** The Google connector holds one account; a second sign-in **replaces** it rather than adding. Gmail, Calendar and Drive ride the same sign-in and move together. Google and Microsoft can coexist (`inferred` — untested). Two accounts on the *same* provider is where the native connectors stop.

**2. The swap is cheap, which sets the bar (card 065).** Disconnect/reconnect is about a minute and touches nothing but visibility — chats, memory and projects are unaffected. So the constraint only genuinely bites when you need *simultaneous* access.

**3. When a connector earns its place (card 066, `ran-it`).** A specific, recurring, reference-or-update workflow tied to one account. Not inbox triage. Two honest skips: you already manage it natively, or you already have a CLI path — **a connector competes with the access you have, and often loses.**

**4. Scheduling answers the question for you (card 066).** An unattended task needs access live and authenticated at fire time. Which raises the stakes on §2.3.1 considerably — see the gotcha below.

**5. What a plugin actually installs (card 069, `ran-it`).** Three things, not four: skills, connectors, and the plugin that bundles them. Commands are merged into skills; the trigger is a frontmatter flag. **Only the connector holds credentials.** Installing Anthropic's Data plugin adds *ten* namespaced skills, not one thing called Data — and every installed skill costs its listing every session (card 059).

### The gotcha worth the whole hour (card 067, `inferred`)

Swap the connector on Tuesday to check a personal inbox, forget to swap back, and Wednesday's scheduled task either fails or **quietly succeeds against the wrong account**. In live chat you'd catch it instantly; under a schedule nobody is watching.

> The dangerous outcome is the success, not the failure.

State plainly that this is reasoned, not observed, and that one question decides it: does a schedule resolve the connector at fire time, or pin it at creation? Until that's answered the mitigation is to treat a scheduled account as dedicated and have the task assert its own context before writing.

### Lab (~25 min) — connect, scope down, and count the cost

1. Connect one Google account; turn **off** Gmail and Calendar, leaving only Drive. Minimum scope on a consumer surface (card 039).
2. Install a plugin, then open the Skills tab and find the namespaced components it added.
3. `claude plugin details <name>` — read the always-on vs. on-invoke token cost aloud.
4. Uninstall it and note what the session stops carrying.

### Takeaway

Audit what you have connected. For each one, name the recurring task that justifies it. Disconnect anything that fails the test.

---

## Labs — one built, one gated

| Lab | Card | Status |
|---|---|---|
| Connect, scope down, count the cost (Topic 2.3) | 065, 066, 069 | **Runnable** — all three cards are `ran-it` |
| Publish an artifact, open it as someone else (Topic 2.2) | 029, 030 | Gated — both cards are `docs`; the lab *is* the test |
| Scheduled morning brief — calendar → Gmail → a Drive doc per meeting | 068 | **Gated by design.** Four steps unverified; `labs/README.md` bars building it until they're run |

Card 068 is filed as a design, not a lab, on purpose: it would otherwise teach a scheduled task using connectors, writing to Drive, and searching Gmail unattended — none of which has been run. One sitting clears all four and card 067 with them.

---

## What's verified and what isn't

| Claim | Card | Status | Note |
|---|---|---|---|
| Two-session split (seminar chat + code session) | 001 | `ran-it` | Months of use; 340+ cards produced |
| Passive vs. active retrieval axis | 011 | `ran-it` | MCP and CLAUDE.md halves; Projects half is `docs` |
| Triggered pointers beat standing instructions | 014, 016 | `ran-it` | Behaviour confirmed repeatedly; the *mechanism* is inferred |
| Projects: 5-project cap on free tier | 007 | `docs` | Paid-tier limits are **undocumented** — that absence is the claim |
| Automatic RAG near capacity | 007 | `docs` | Threshold not stated; "approaches capacity" is vague |
| CLAUDE.md re-injected after compaction | 017 | `docs` | Not yet survived a real weeks-long session |
| Artifacts have no path back into Projects | 027 | `docs` | **Shortest shelf life in the corpus** — a negative claim goes false silently |
| Artifact publishing auth gate | 028 | `docs` | Version-pinned; re-check |
| Connector-backed artifacts run as the viewer | 029 | `docs` | Not run; the lab above is the test |
| One account per provider; second sign-in replaces | 065 | `ran-it` | Google+Microsoft simultaneity is `inferred` |
| Connector swap is ~1 min and non-destructive | 065 | `ran-it` | Sets the bar for building any multi-account workaround |
| Skill/connector/plugin taxonomy | 069 | `ran-it` | Commands merged into skills; trigger is a frontmatter flag |
| Plugin token cost is reportable | 069 | `ran-it` | `claude plugin details` |
| Scheduled tasks drift with a swapped connector | 067 | `inferred` | **Nobody has hit it.** One open question decides whether it's real |
| A connector competes with an existing CLI path | 066 | `ran-it` | Standing choice, not a measurement |

Two of these are **short-shelf-life product claims** — 027 and 028. They should carry a re-check date, because if either ships a change, nothing in the corpus will error; the lecture will just quietly be wrong.

---

## Gaps — where more hands-on time is needed

Ranked by what most limits the section.

**1. Mobile is structurally missing.** This is the biggest one. Cards touch iOS only in passing (032, 052), yet mobile is where a lot of the actual operating happens — and where this corpus's own author works. There's no carded account of how context, Projects, connectors, and artifacts behave on the phone. *Needed: a week of deliberate mobile-first use, carded.*

**2. Connector setup has no how-to.** Cards 011, 029, and 038 discuss connectors as a context mechanism but never walk through configuring one, or what failure looks like when credentials lapse. A CoE lead asking "point a Project at our shared Drive — how?" gets no answer.

**3. Team and Enterprise surfaces are unexplored.** Who can publish, who can revoke, what org-scoped sharing actually means, what the audit trail looks like. Card 012 says "upload a subset to a Project" without the governance model around it. This is squarely the CoE audience's job and the corpus can't yet speak to it.

**4. No decision tree.** The material implies one — one-off question, recurring research, code-heavy work, collaborative viz, mobile — but never states it. That artifact is cheap to build and probably the single most useful handout in the section.

**5. No cost visibility across surfaces.** What chat costs versus Projects versus Claude Code versus API, what's visible in billing, whether limits are per-user or org-wide. Budget questions arrive early from this audience.
