# Re-Injecting State After Compaction — `SessionStart` with the `compact` Matcher, and Why CLAUDE.md Doesn't Need It

**Source:** Dave, Claude Boot Camp session 2026-09-11, wanting a post-compaction hook to re-read CLAUDE.md on sessions he leaves open for weeks. Mechanics corrected by Beta, then verified 2026-09-11 against the official Claude Code docs via a research pass:
[hooks reference](https://code.claude.com/docs/en/hooks.md) · [hooks guide](https://code.claude.com/docs/en/hooks-guide.md) · [memory](https://code.claude.com/docs/en/memory.md) · [context window](https://code.claude.com/docs/en/context-window.md). The "CLAUDE.md is re-injected from disk" and "SessionStart `compact` output is added to the compacted context" claims both come from the *what survives compaction* table in those last two.
**Type:** pattern
**Verified:** docs — every claim below confirmed in official documentation at code.claude.com/docs. **Not yet run** in Dave's setup. `[confirm-this: needs a real weeks-long session with multiple compactions before this backs a lab.]`
**Relevant to:** 5 (building with Claude Code), 2 (operating Claude products)

## The correction that matters most

**CLAUDE.md is already re-injected from disk after compaction.** No hook required. The docs' "what survives compaction" table lists project-root CLAUDE.md and unscoped rules as re-injected automatically.

So the original instinct — *"I need a hook to remind Claude to re-read CLAUDE.md"* — is solving a problem the harness already solves. Build that hook and it's dead weight.

What *doesn't* survive is everything dynamic: git state, deploy status, anything about the world that changed while you worked. That's the gap worth filling, and it's the same gap as card 014.

## The verified mechanics

There is **no dedicated post-compaction hook event**. The mechanism is `SessionStart` with the `compact` matcher.

Valid `SessionStart` matchers:

| Matcher | Fires when |
|---|---|
| `startup` | New session launched |
| `resume` | Resumed from a saved session |
| `clear` | After `/clear` |
| `compact` | **After context compaction** |
| `fork` | Forked from another session |

Combine with `|`, e.g. `"startup|resume|compact"`.

`PreCompact` fires *before* compaction and **cannot** inject context — it can block compaction or emit a system message, but anything it added would just be swallowed by the summarization that follows. It's for logging and side effects.

### Configuration

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "startup|resume|compact",
        "hooks": [
          { "type": "command", "command": "./.claude/hooks/reload-state.sh" }
        ]
      }
    ]
  }
}
```

The hook writes JSON to **stdout**:

```json
{
  "hookSpecificOutput": {
    "hookEventName": "SessionStart",
    "additionalContext": "Branch: main\nLast commit: abc1234 Fix the thing"
  }
}
```

A minimal script:

```bash
#!/bin/bash
BRANCH=$(git rev-parse --abbrev-ref HEAD 2>/dev/null || echo "no git repo")
LAST=$(git log -1 --pretty=format:"%h %s" 2>/dev/null || echo "no commits")
jq -n --arg b "$BRANCH" --arg l "$LAST" \
  '{hookSpecificOutput: {hookEventName: "SessionStart",
    additionalContext: "Branch: \($b)\nLast commit: \($l)"}}'
```

## The caveat for weeks-long sessions

**Injected context does not persist across the *next* compaction.** The hook re-runs on each one and must produce correct output every time. So the hook must be **deterministic and idempotent** — read live state at run time, never echo a stale cached blob. A hook that prints a file written weeks ago will confidently re-inject something false after every compaction, which is worse than injecting nothing.

## Why this matters

This is the **deterministic fix for card 014**. There, a standing `CLAUDE.md` instruction to re-verify git state loses to the model's trust in its own snapshot, because the instruction has to win an argument every turn. A hook doesn't argue: the harness runs it, and the true state arrives as context whether or not the model thought to look.

That's the general shape worth taking away: when "sometimes the model forgets" is unacceptable, move the check from *instruction* to *harness*. Hooks are the place determinism lives — and one of the concrete answers to the open question in card 010.
