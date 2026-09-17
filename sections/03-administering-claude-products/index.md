---
title: "3. Administering Claude Products"
order: 3
status: drafting
---

## Scope

The center-of-excellence job. What you own once more than a handful of people are using this.

**Say the honest thing up front.** This section is not a separate body of knowledge. It is a **governance lens applied to the other sections' material** — the same surfaces from §2, the same integrations from §4, the same Salesforce work from §6, asked a different question: *who is this acting as, and what happens when it's wrong?*

That's not a weakness, and it should be named in the room rather than papered over. For a CoE lead the section's promise is **"how to think about the decisions you're already making,"** not "here's a new skill." Pretending otherwise invites the audience to look for a domain that isn't there.

It also absorbs a cluster of cards originally filed under §1 (034, 042, 044, 045, 046, 047) — those are identity and access material, not foundations.

---

## The sequence

The cards were filed as answers to scattered questions, which makes the section read as a reference guide. Taught, it needs to be a **runbook in order**:

1. Choose the access model (046, 035)
2. Bound the blast radius (039, 026)
3. Scale credential management (041, 042, 043)
4. Enforce the constraints (050, 033)
5. Audit and measure — **missing, see gaps**

---

## Topic 3.1 — Two access models, and picking one

*One hour. Lecture ~25 min, lab ~25 min, takeaway ~10 min.*

### The spine

When Claude acts, it either uses **your** credentials or **its own**. Anthropic ships both models deliberately, and the choice determines everything downstream.

### Lecture arc

**1. Connectors mirror your permissions (card 046, `docs`).**
Claude acts as you. Admins can *narrow* what it does but never grant more than the source system already permits — restricting actions in Claude never expands access. Right for single-player work.

**2. Agent identity gives Claude its own accounts (cards 046, 037, `docs`).**
Claude Tag provisions per-system service accounts — different keys at different permission levels per channel. Right for shared, autonomous, multi-person work, where acting as any one person is a side door into that person's private documents.

**3. Blast radius is bounded by the credential, not the tool (card 039, `ran-it`).**
A scoped service token is bounded by something *you configured*. Ambient user permissions are bounded by something *you accumulated over years*. If you're an admin on production, "bounded by what I can do" is not a bound. This single sentence is the most portable thing in the section.

**4. Cowork ships the scoping layer you'd otherwise hand-roll (card 045, `docs`).**
Role-based access, per-tool connector permissions, group spend limits, OpenTelemetry logging — aimed at the non-engineering majority. Hand-built MCP still wins where "read-only token, four known people" is the correct blast radius.

**5. Cost *shape* changes behaviour, not just the bill (card 035, `inferred`).**
Subscription plans with rolling caps make marginal cost feel like zero and keep exploration cheap. Per-token metering makes every question a small decision, so people batch, hesitate, and skip the exploratory follow-up — which is the behaviour that made the tool valuable. **Teach this as the reasoning being tested, not as measured fact.**

**6. And none of it is new (card 044, `inferred`).**
Least privilege, token scoping, revocation, per-user identity. Ordinary architecture, applied faster and wider. Lead with the principle, then show it landing on MCP, on connectors, on Claude Tag — so it reads as application rather than invention.

### Lab (~25 min) — prove a boundary you believe in

Set up two credentials against a sandbox system — one read-only, one broad. Point Claude at each in turn and ask for the same write operation. Watch one refuse and one succeed.

**Access caveat, stated before the cohort starts:** this only teaches anything if each learner controls a sandbox. Card 039's ambient-credential half additionally needs them to be an admin somewhere. Plan for learners who have neither, or the lab proves nothing.

### Takeaway

Before anything else: *whose permissions, and how did they get that wide?*

---

## Topic 3.2 — Credentials as they scale

*One hour. Lecture ~20 min, lab ~30 min, takeaway ~10 min.*

Largely shared with §4.2 — teach whichever the audience needs. Engineers building integrations get it in §4; CoE leads setting policy get it here, with the emphasis on **when to switch tiers** rather than how to implement.

### Lecture arc

**1. Small-team practice is legitimate (card 041, `ran-it`).** `.env.example` plus a password manager, scoped to the project's people, with individual PATs where attribution matters. The ceiling is support burden.

**2. Disposable environments are non-negotiable (card 026, `ran-it`).** Claude gets a scratch org, at least until the gotcha corpus exists. The `sf` CLI is powerful enough to damage production, and a mistake in a disposable environment costs a rebuild rather than an incident.

**3. Centralising relocates attribution (card 042, `inferred`).** It moves from the credential to your application logs. Build the logging in the same change, or it's gone. Note that card 046 reports Anthropic solved this as a product feature in agent identity — so the DIY burden may be smaller than 042 implies.

**4. Determinism is a governance instrument (card 033, `inferred`).** When an output drives a decision someone will later question, a deterministic rollup beats a model — not because it's more accurate, but because it's wrong the same way every time, and that's what makes it defensible and fixable.

### Lab (~30 min)

The MCP logging lab from §4.2 serves here too: make the server log every call, then reconstruct the agent's behaviour from logs alone and compare with its own narrative.

---

## What's verified and what isn't

Governance advice that's wrong is expensive, so this table is deliberately harsh.

| Claim | Card | Status | Note |
|---|---|---|---|
| Whose *account* a connector holds (one per provider) | 065 | `ran-it` | Multiplicity companion to 046 |
| Unattended tasks inherit mutable connector state | 067 | `inferred` | **Nobody has hit it**; one question decides it |
| Distribution axis is the org boundary, not plan tier | 060 | `n/a` stance | Team-marketplace claim is `docs`, untested |
| Anonymous surfaces can only screen; access needs identity | 061 | `inferred` | Reasoned from a single observed demo |
| Blast radius bounded by credential | 039 | `ran-it` | Strongest claim in the section |
| Disposable org for Salesforce work | 026 | `ran-it` | |
| `.env` + password manager to ~10 people | 041 | `ran-it` | |
| Connectors mirror permissions | 046 | `docs` | Direct from official docs |
| Agent identity / per-system service accounts | 046, 037 | `docs` | Not run |
| Cowork admin controls | 045 | `docs` | Not run |
| Cost shape changes behaviour | 035 | `inferred` | **A bet, not a finding.** No telemetry, no study |
| Determinism buys debuggability | 033 | `inferred` | Principle; no worked incident behind it |
| Attribution must be rebuilt in app logs | 042 | `inferred` | Real at scale; oversold for small teams |
| Token exchange | 043 | `inferred` | **Not built** |
| Salesforce-in-Claude will be metered | 034 | **prediction** | Dave's bet. Pricing unpublished |
| Trust machinery bundled into Agentforce price | 036 | `inferred` | Not confirmable in pricing docs |
| Custom/enterprise connector credential model | 045, 046 | **open** | Changes the risk profile either way |

**Do not teach cost comparisons yet.** Cards 034, 035, and 036 together form the cost argument, and all three are inferred or predicted. If Salesforce-in-Claude prices free-on-subscription, the argument inverts.

---

## Gaps — where more hands-on time is needed

This section has the widest gap between *principles carded* and *execution carded*. It teaches someone to recognise good governance; it does not yet teach them to build it in a real org — which is exactly what a consultant has to do repeatedly for different clients.

**1. No week-one rollout playbook.** The cards assume a team and a governance need already exist. A 5,000-person company says "we want Claude" — what does the CoE lead do first? The minimal operable setup, standing up *before* usage arrives, is uncarded and is arguably the single most valuable thing this audience wants.

**2. No decision tree across the options.** Claude Tag vs. DIY MCP vs. Cowork connectors vs. a hand-built integration. Cards 034, 045, and 046 circle this without landing it. One card that says *"non-technical users + admin-scoped work → Claude Tag; engineers + production blast radius → DIY MCP; connectors with stricter admin control → Cowork"* would carry a large share of the section.

**3. No audit walkthrough.** For a regulated org an audit asks: which human initiated this, was it permitted, what data was touched, has that access since changed? Cards 039 and 042 cover fragments. No card shows a complete audit trace of a Claude-assisted operation.

**4. No escalation trigger.** Card 041 says the password-manager approach breaks around ten people. *Then what* — Vault? split the team? move to Claude Tag and offload it? The tiers are carded; the switching decision isn't.

**5. No cost containment patterns.** Hard caps vs. soft warnings, per-team budgets, attribution to business units, sunsetting an expensive pattern. Card 048 is about *where* to spend intelligence, which is a different question from *how much is exposed*.

**6. The governance cluster has no lab at all.** Every card in 034–047 is `docs` or `inferred`. Under the project's own rule, none of it can be labbed. Running Cowork or Claude Tag once — even a small setup — would unlock the section's hands-on half.

**7. Nobody has priced governance against the risk it prevents.** OAuth token exchange is non-trivial to build. For a twenty-person company, is that cost higher than the exposure it removes? The curriculum should ask this out loud rather than implying more governance is always better.
