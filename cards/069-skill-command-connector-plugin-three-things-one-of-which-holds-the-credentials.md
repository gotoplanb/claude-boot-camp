# Skill, Command, Connector, Plugin — and Why That's Three Things, Not Four

**Source:** Dave in Claude for Mac, 2026-09-17, installing Anthropic's **Data** plugin and finding both a "Data" plugin and a data skill in Customize: *"are they like the same thing?"* Beta confirmed the bundle contents against its own live skill list, same session. **Revised 2026-09-17** after Dave built a real plugin, now published at [gotoplanb/claude-plugins](https://github.com/gotoplanb/claude-plugins) (`plugins/release`) — see the correction below.
**Type:** product-behavior
**Verified:** `ran-it` for everything in the corrected taxonomy — a working plugin was authored, installed and invoked. The Data-plugin enumeration is `ran-it` from Beta's skill list.
**Relevant to:** 2 (operating Claude products), 4 (integrating), 5 (building with Claude Code), 1 (foundational concepts)

## Correction — this card originally said four things

The first version of this card presented **skill** and **command** as two primitives *"split by who decides."* Building a plugin showed that's wrong on the mechanism:

> *"Custom commands have been merged into skills. A file at `.claude/commands/deploy.md` and a skill at `.claude/skills/deploy/SKILL.md` both create `/deploy` and work the same way."*

They are **one primitive**. The who-decides insight survives — it just isn't a file-type choice.

## The three things

**Skill** — an instruction file. It can be loaded two ways, and **frontmatter decides which**:

```yaml
disable-model-invocation: true   # never auto-fires; user triggers /name only
```

- Flag absent → Claude loads it when it infers relevance.
- Flag present → the model can't fire it; you invoke it explicitly as `/name`.

That flag *is* the "command vs. skill" decision. One knob, not two file types. (Verified at runtime: the flag blocks model auto-firing but not explicit `/name` invocation.)

**Connector** — the OAuth/MCP wiring to an external system. **The only one of the three holding real credentials and reaching live data.** Skills are inert without it — usable by pasting results in by hand, but nothing gets queried. When asking "what can this actually touch," the answer is always the connector.

**Plugin** — the packaging layer. Manifest at **`.claude-plugin/plugin.json`** (*not* root `plugin.json` — the original card had this wrong), with `skills/` at the plugin root. Installs as one unit.

## A skill is not "just markdown" — corrected

The original card flagged this as unverified and hedged toward markdown-only. **That's wrong.** A real `SKILL.md` carries:

| Frontmatter | Does |
|---|---|
| `disable-model-invocation` | The trigger knob above |
| `allowed-tools` | Declares required tools, e.g. `Bash(git log:*), Read, Edit, Write` |
| `argument-hint` | Declares arguments, consumed as `$1` |
| `description` | What the model matches on when auto-firing is allowed |

…and the body can contain **executable blocks** (```` ```! ````) whose output is injected before the model reads the file. Skills execute; they aren't inert text.

## Namespacing and what a plugin install actually does

Installing "Data" didn't add one thing called Data. It added **ten** skills — `data:analyze`, `data:data-visualization`, `data:build-dashboard`, `data:write-query`, `data:statistical-analysis`, `data:sql-queries`, `data:create-viz`, `data:explore-data`, `data:validate-data`, `data:data-context-extractor` — plus slash commands and connector definitions for three warehouse types, in one install.

The Skills tab wasn't showing a duplicate of the plugin. It was showing **the components the plugin installed**, which is why they're all prefixed. The namespace is **plugin `name` + skill `name`**, so the release plugin's `skills/patch/SKILL.md` is invoked as `/release:patch`.

A consequence worth knowing: **don't add a paired `commands/` file for a skill you've already named** — they'd collide on the same `/name`.

**This also refines card 037**, which calls plugins "bundles of skills." They bundle connector wiring too, and that's the part that matters for access.

## Bundle or cherry-pick

Every installed skill costs its listing in context every session, used or not (card 059). Ten skills' listings is a small standing tax, but permanent — and if you installed for one of the ten, you carry nine you'll never fire.

- **Bundle** when the domain matches something you do repeatedly and you'd reach several pieces.
- **Cherry-pick** when you wanted one capability and the rest is someone else's workflow.

## Why this matters

The operational distinction is **which one holds credentials**: you can audit a skill by reading it; you cannot audit what a connector reaches without checking the account behind it (cards 046, 065).

And the trigger question — *should the model be allowed to fire this on its own?* — is now a concrete, teachable knob rather than an architectural choice. That's card 070's lab in one line.

See card 070 (the plugin that corrected this card), card 071 (`allowed-tools` as the declaration mechanism), card 059 (context cost), card 060 (plugin marketplaces), cards 046/065 (whose permissions, whose account), and card 072 (why the judgment survived while the mechanism didn't).
