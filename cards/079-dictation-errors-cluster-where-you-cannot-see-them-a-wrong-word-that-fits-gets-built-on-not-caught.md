# Dictation Errors Cluster Where You Can't See Them — a Wrong Word That *Fits* Gets Built On, Not Caught

**Source:** Dave, 2026-10-05, scanning his own published walking-tour transcript ([The Monument Tells You What Survived](https://davestanton.com/blog/the-monument-tells-you-what-survived)) and finding a word he hadn't said. The gap-filling reading is his; the clustering framing came out of the same exchange.
**Type:** gotcha
**Verified:** `ran-it` for the instance — a real transcript, a word Dave confirms he didn't say, built on by both parties for the rest of the conversation without either noticing. `inferred` for the mechanism: modern speech-to-text is context-conditioned, so the error *plausibly* arose that way, but nothing here measured it. *[confirm-this: is the effect strong enough to reproduce deliberately — dictate a sentence whose key word is contradicted by the preceding context and see which way it resolves.]*
**Relevant to:** 2 (operating Claude products), 1 (foundational concepts), general

## The instance

Mid-conversation, Dave dictated that he was **morally neutral** about culture — looking at cultural spread historically rather than judging it.

It transcribed as: *"I'm pretty **materialist** about it."*

He had just said, in the same breath, that humans are *"meat sack robots compelled by electrical and chemical feedback loops."* Which is materialism. So the wrong word was **exactly as coherent as the right one**, the assistant built on it, Dave didn't catch it, and the conversation ran on that footing for the next several thousand words.

He only found it months later, scanning the transcript to publish it.

## Why this is a different failure from card 006

Card 006 covers dictated **proper nouns** going wrong — "Claude for iOS" becoming "Quadfly OS." Those are catchable: the term is a non-thing, and the sentence around it points at the gap.

This is the inverse, and the causes are opposite:

| | Card 006 | This card |
|---|---|---|
| What breaks | Proper nouns, file names, model names | Ordinary words |
| Why | Speech-to-text has **weak** priors on your domain's vocabulary | It has **strong** priors, conditioned on what you just said |
| What you get | A word that doesn't exist or doesn't fit | A word that fits perfectly |
| Detectability | High — it reads as wrong | **Near zero** — nothing reads as wrong |
| Fix | Glossary, custom vocabulary, rename the thing | None of those help |

A glossary can't catch this. There's nothing to look up. The transcription isn't failing to recognise a term — it's succeeding at predicting a plausible one.

## The clustering claim

> **Dictation errors are not uniformly distributed across the space of wrong words. They're concentrated where the conversation has already made a wrong word plausible.**

The transcription model is conditioned on context. The more coherent your conversation, the more coherent its errors. So the error rate you *perceive* is much lower than the real one, because the detectable errors and the actual errors are different populations.

The ones you catch are the ones that don't fit. The ones that fit, you build on.

## It cuts both ways — this is the useful half

The same property is genuinely valuable, and the card would be dishonest without it:

> **When you know the idea but can't pull the word out of memory, context-conditioned transcription fills the gap for you — often with the right word, precisely because it's predicting from everything you just said.**

Dictating while walking is exactly the condition where retrieval is worst and the idea is clearest. The mechanism that produced the error is the one that makes dictation usable at all.

So it isn't a bug to suppress. It's a bias to **know you're operating inside.**

## What actually mitigates it

Not vocabulary. The only defences are adversarial:

- **Push back on yourself.** Re-read your own turns, not just the replies. The error is in *your* half, which is the half nobody proofreads.
- **Push back on the assistant.** A model that agrees smoothly with a word you didn't say will keep agreeing. Card 058's discipline — ask to be challenged, and mean it — is the working defence here, not a glossary.
- **Expect coherence to be weak evidence.** "It read fine" is what the failure looks like from the inside.

## Why this matters

It inverts the intuition that fluent output means accurate input. For a voice-driven workflow — and for anyone teaching one — the lesson is that **transcription quality cannot be assessed by reading the transcription**, because the failures are selected for reading well.

It also sharpens what an adversarial partner is *for*. Not just catching bad reasoning, but catching the premise that was never actually stated.

See card 080 (the other way a published transcript misrepresents the human — there the words are right and the volume is wrong), card 006 (the proper-noun class this sits beside — opposite cause, opposite detectability), card 058 (ask to be challenged, and mean it — the only real mitigation here), card 014 (building confidently on a stale or wrong picture of the world), card 022 (observe reality rather than accepting the report), and card 077 (the same shape elsewhere: optimizing against what you measured and silently discarding what you didn't).
