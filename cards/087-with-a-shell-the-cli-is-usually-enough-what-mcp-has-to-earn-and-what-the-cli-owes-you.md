# With a Shell, the CLI Is Usually Enough — What MCP Has to Earn, and What the CLI Owes You

**Source:** Dave, working notes for a Salesforce automation engagement, 2026-11-01.
**Type:** preference
**Verified:** `ran-it` for the engineering practices — standard discipline Dave applies to his own tools, and the context-cost half is measured (card 069: `claude plugin details` reports it). `inferred` for the stdio-MCP-as-a-subcommand arrangement specifically, which is a design he hasn't built.
**Relevant to:** 4 (integrating — MCP and APIs), 5 (building with Claude Code), general

## The decision

> **With a shell available, the CLI alone is usually enough.** Tool definitions cost context whether or not they're used.

Card 059 priced that: an MCP server's schemas are resident from connect time. Card 075 explains why a CLI escapes it — `--help` is retrievable on demand, and the model already has strong priors on CLI conventions. So a CLI is the cheaper interface by default, and MCP has to argue for itself.

**MCP earns its place when:**

- There's **no shell** in the environment.
- You need **persistent state** across calls.
- You want **typed results** rather than text to parse.
- You need **finer permission control** than "can run commands."

Absent one of those, a CLI does the same job for less standing cost.

**And it isn't either/or.** The same CLI can expose an MCP face as a subcommand over stdio, started inside the session — nothing to host, nothing installed on anyone's machine when sessions run in cloud environments (card 082). One implementation, two front doors, and the MCP one only costs context where it's actually turned on.

## What the CLI owes you

A CLI an agent drives is a different artifact from one a human drives. It needs:

- **Predictable exit codes.** The agent's primary signal.
- **JSON output.** Don't make it parse prose you'll reword later.
- **A dry-run mode.** Prerequisite for card 084's attended runs.
- **Idempotence.** Agents retry. A second run must be safe.
- **A `doctor` command** that checks tool versions, auth, and network reachability — because "it didn't work" has a dozen causes and most are environmental. In a cloud sandbox this is the first thing you run.

## Engineering notes

- **Wrap existing tools, don't reimplement them.** You're building orchestration, not a replacement for `sf` or `git`.
- **Put one seam at the subprocess boundary** so recipe logic is unit-testable against a fake runner. One seam, deliberately placed, is what makes the rest testable without a live org.
- **Keep logic out of Makefiles.** A Makefile that grew conditionals is a program in a language chosen by accident.

## Why this matters

Most of this is ordinary CLI craft, which is the point — card 044's observation again, that almost none of this is AI-specific. What *is* specific is the weighting: exit codes, JSON and idempotence matter more than usual because the caller can't improvise around a surprise, and the context cost of a tool definition is a real recurring price that a `--help` page doesn't charge.

See card 059 (what tool definitions cost), card 075 (why a CLI is progressive-disclosure-shaped), card 060 (the other MCP decision — reach, not cost), card 086 (the package this CLI sits inside), card 084 (what dry-run and idempotence are for), and card 044 (ordinary engineering discipline, bigger blast radius).
