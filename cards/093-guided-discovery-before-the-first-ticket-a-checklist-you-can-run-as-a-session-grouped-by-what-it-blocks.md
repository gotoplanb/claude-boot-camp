# Guided Discovery Before the First Ticket — a Checklist You Can Run as a Session, Grouped by What It Blocks

**Source:** Dave, working notes ahead of a Salesforce automation engagement, 2026-11-01. Written so the checklist can be pasted into a session and used to run the kickoff interview.
**Type:** pattern
**Verified:** `inferred`. The checklist is reasoned from the design in cards 081–091 and standard Salesforce implementation prerequisites. **It has not been run on a real engagement.** *[confirm-this: after the first kickoff, record which questions were missing, which were unnecessary, and which answers changed the plan.]*
**Relevant to:** 6 (Claude + Salesforce), 5 (building with Claude Code), 3 (administering), general

> **Relationship to card 092.** That card is the *lens* — which answers are go/no-go and which merely shape. This one is the *instrument*: the full interview, grouped by what each answer blocks. Use 092 to know which answers are fatal; use this to actually run the kickoff.

## The idea

Discovery questions are cheap to ask and expensive to discover late. Group them by **what each answer blocks** (card 057's move), so the list doubles as a plan: you know which questions must be answered before day one and which can wait.

> **A discovery list is a dependency graph in disguise. Ask the questions that block the earliest work first.**

## How to run it as a session

Paste the checklist below into a session and ask Claude to interview you — or the client contact, if they join — one group at a time. Ask it to:

- Record each answer next to its question, and mark anything unanswered as open.
- Flag which open items block which phase.
- Stop at the end of each group with a short summary of what is known and what is still blocking.
- **Not guess at answers.** An unanswered question stays unanswered.

The point of using a session is that you don't have to remember the list, and the output is a record you can hand back to the client.

## The checklist

### Group 1: Blocks day one (access and environment)

- [ ] A refreshed sandbox that matches production. What is its type, and what is its refresh interval?
- [ ] Does the sandbox contain copied production data? (Affects what can safely appear in screenshots and logs — card 088.)
- [ ] Read access to production metadata so we can capture the baseline into source control.
- [ ] System administrator profiles for the developers who set things up.
- [ ] Is Dev Hub enabled on the production org? What are the daily and active scratch org limits for this edition?
- [ ] Who owns the GitHub organization the repository will live in, and who can grant access?
- [ ] A Jira project to work in, and an API credential for it. Who issues it?

### Group 2: Blocks the first recipes (how they work today)

- [ ] How are changes made today? (Change sets, direct edits in Setup, something else.)
- [ ] Is anything already in source control, even partially?
- [ ] Who approves a deployment to production, and how long does that take?
- [ ] Are there change windows, freezes, or compliance rules that constrain deployment?
- [ ] Which ticket types come up most often? (These become the first recipes. Start with the simplest — fields, lookups, cross-object formulas.)
- [ ] What do admins do in Setup that is **not** captured as metadata? (These are the hidden prerequisites that block unattended runs — card 084's criterion 2.)

### Group 3: Blocks observability choices

- [ ] What do you use today for logging and monitoring, if anything?
- [ ] Who would look at run data, and how often?
- [ ] What would you want to know about the agent's work that you can't see now?
- [ ] Which Salesforce-side records are available to them already? (Audit trail, deployment history, debug logs, event monitoring — depending on licensing.)

### Group 4: Gates for phase two (admins working in cloud sessions)

These do **not** block phase one, while the only users are developers pulling from GitHub.

- [ ] Can admins use cloud sessions under the customer's enterprise settings?
- [ ] Are repo-level plugins permitted? (Go/no-go for the distribution model — card 090.)
- [ ] Can environments hold the secrets each role needs, and can the requester-facing environment be kept free of org credentials?
- [ ] Does the customer's GitHub licensing include code and secret scanning on private repositories?
- [ ] Does the customer approve the tools the CLI would depend on?

### Group 5: Handover

- [ ] Who owns the CLI and the recipes after the engagement ends? (Card 090: a customer can depend on a CLI without being able to support it.)
- [ ] Who is allowed to approve edits to recipes, and through what review?
- [ ] Who are the admins who will review finished work, and what do they need to see to accept it?

## What to do with the answers

- **Anything unanswered in Group 1 is a blocker.** Get it before work starts.
- **Group 2** decides which recipes to build first, and which hidden prerequisites need writing into them (card 084).
- **Group 3** decides whether the vendor-neutral telemetry design (card 089) needs any backend at all. Start from their stated needs, not a preferred tool.
- **Group 4 and 5** shape the rollout plan. Record them even when they are not urgent.
- **Re-run the checklist after phase one.** Several answers will have changed — and card 056's warning applies: a restriction recorded once tends to outlive the constraint that justified it.

## The takeaway

> **Run discovery as a session with a checklist grouped by blocker. You get a plan, a record, and a way not to blank on the day.**

The phase-two grouping is the part that earns its place. Four of these questions look like blockers and aren't — while developers are pulling from GitHub, enterprise cloud-session settings don't gate anything. Sorting by what each answer actually blocks is what stops a "no" in Group 4 from reading as a reason not to start.

See card 092 (which of these are fatal and which merely shape), card 057 (group open questions by blocker — the same move), card 084 (recipe graduation and hidden prerequisites), card 085 (the baseline drifts), card 086 (recipe intake questions — the ticket-level cousin of this card), card 088 (why the production-data question matters), card 089 (telemetry design), card 090 (the repo-plugin precondition and the handover problem), and card 091 (what ships where, and why).
