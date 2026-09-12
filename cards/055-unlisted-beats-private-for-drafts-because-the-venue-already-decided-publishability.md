# Unlisted Beats Private for Drafts — Because the Venue Already Decided Publishability

**Source:** Dave, Claude Boot Camp session 2026-09-12, after I flagged that davestanton.com drafts are unlisted rather than private and assumed that was a gap.
**Type:** preference
**Verified:** `ran-it` — davestanton.com works this way. `published: false` drops a post from the index; `/blog/<slug>?drafts=true` renders it with no authentication.
**Relevant to:** general, 5 (building with Claude Code)

## The mechanic

Drafts on the site have `published: false` in frontmatter. That removes them from the index and makes the bare URL return 404. Add `?drafts=true` and the post renders — **no login, no token, no check.** Anyone holding the URL can read an unfinished draft.

Read as an access control, that's a hole. It isn't one, and the reason is a heuristic worth stating explicitly.

## The heuristic

> **If it doesn't belong on the public site, it doesn't go on the public site.** Given that it's there at all, unlisted is sufficient.

In Dave's words: *"if I was publishing something that was super private, I probably wouldn't put it on my website anyway, and this is a good heuristic of whether it deserves to be on my website. I'd be fine with it being unlisted — and if a handful of friends happen to come across it, I wouldn't care."*

The consequence is the part that's easy to miss: **`?drafts=true` performs no security function whatsoever.** The filtering already happened, upstream, at the moment something was placed in a public venue rather than the private annex (card 053). By the time content reaches the site, it has been judged publishable. An access control downstream of that judgment is protecting nothing, because there's nothing left to protect.

## So what is "unlisted" actually for?

Not protection — **curation**. Keeping unfinished work out of the index means the front page represents the finished body of work rather than every half-written thing in progress. It manages *attention*, not *access*.

That distinction is the whole card. Obscurity is:

- **Fine as curation.** Nobody is harmed by having to know a URL to find a rough draft.
- **Fatal as protection.** URLs leak — pasted into chats, captured in referrers, fetched by crawlers, shared by the very share sheet this site puts on every post.

The common failure is using obscurity for the second job because it's convenient for the first. Someone puts a genuinely sensitive document behind an unguessable URL, reasons that nobody will find it, and is right until they aren't. This heuristic makes that impossible by construction: sensitive material never enters the venue where unlisted is the only control.

## What makes it workable

A low cringe threshold (card 053). Unlisted drafts are only comfortable if being caught mid-thought doesn't bother you — *"I don't really mind if I publish work in process and it's a little bit messy or garbled or incoherent."* Someone who would be mortified to have a rough draft read needs a real access control, or needs to draft somewhere else entirely.

So this isn't advice that generalises on its own. It's downstream of the public-by-default posture in cards 053 and 054. **The heuristic is cheap because the hard decision was already made somewhere else.**

## Why this matters

Two questions get collapsed into one and shouldn't be:

1. *Should this be publicly readable?* — a judgment, made once, at placement.
2. *How hard is this to find?* — a presentation choice.

Answer the first honestly and the second stops carrying weight it was never built to carry. Answer it vaguely — "it's kind of private, I'll just not link it" — and you've asked a URL to be a permission system.

A related observation from the same session, kept because the framing is useful even though the specific case is unresolved: a `git push` of this corpus was blocked by a safety classifier while **the same content was already live on the public website through a submodule.** Whatever actually triggered that block is unknown and probably unrelated to publishability — but it's a clean illustration of a control applied to a mirror of something already public. `[confirm-this: the cause of that block was never diagnosed; don't cite it as more than an illustration.]`
