# `.env.example` + a Password Manager Is Production-Grade Under ~10 People — and the Ceiling Is Support Burden, Not Security

**Source:** Dave describing how his SRE team handles secrets, Claude Boot Camp session 2026-09-11. The support-burden reframe is Beta's, same session. Resolves the open question left in card 039.
**Type:** preference
**Verified:** `ran-it` — this is the live setup for a four-person SRE team, used in production operations, not just prototypes.
**Relevant to:** 3 (administering), 5 (building with Claude Code), 4 (integrating)

## The pattern

1. **`.env.example` committed in every repo** — names which keys are needed and what shape they take. Orientation, not configuration.
2. **The real `.env` lives in the password manager** (LastPass here), shared with exactly the people on that project — four, or fewer depending on scope.
3. **Shared secrets are a starting point.** Individuals then replace some values with their **own personal access tokens**, specifically for systems where you want the log to show *which human* did the thing.

That third step is the one people skip, and it's what makes the whole arrangement auditable rather than merely functional.

## It's not a prototype pattern

Worth saying plainly, because local env files read as amateur-hour to people used to enterprise tooling:

> **Under about ten people this is scalable, secure, auditable, and appropriate for production operations** — not a stepping stone you're supposed to outgrow on principle.

It also preserves fast iteration, which matters when the tooling *is* the work.

## Where it breaks — and why

The failure mode is **not** that it becomes insecure at scale. At 100 people it's just as cryptographically sound as at four. What breaks is **support burden**:

- local environment drift across a hundred machines
- password-manager permission entropy as people join, leave, and move teams
- "works on my machine" MCP servers
- however many OS, shell, and Python-version combinations a hundred laptops represent

You'd be running an unpaid helpdesk. That's the threshold — **operational cost, not risk posture** — and naming it correctly matters, because "we need Vault for security reasons" is usually a worse and less honest argument than "we can't support 100 laptops."

## What you move, and what that buys

Past the threshold: secrets into **AWS Secrets Manager or HashiCorp Vault**, and the compute into the cloud alongside them.

- **Vault/Secrets Manager buys rotation and centralized revocation**, not just storage. The password-manager model works because four people can coordinate *"I rotated the Sumo token, update your `.env`."* At a hundred people that coordination cost alone justifies the move, independent of where compute runs.
- **Moving compute and moving secrets are separate decisions that co-occur for a reason.** Once secrets are centralized, you want the thing consuming them centralized too — otherwise you're fetching central secrets back out to laptops, which undoes the point of centralizing them.

The destination is **internal software treated like SaaS**: one environment you operate, rather than a hundred you support.

## Why this matters

It gives a defensible answer to "shouldn't we be using Vault?" at a stage where the honest answer is no. Adopting enterprise secret infrastructure for a four-person team buys rotation you don't need and costs iteration speed you do.

And it names the actual trigger to watch for. You don't migrate when the team hits a number — you migrate **when you notice you're doing environment tech support instead of the work.** That's an observable signal, unlike "maturity."
