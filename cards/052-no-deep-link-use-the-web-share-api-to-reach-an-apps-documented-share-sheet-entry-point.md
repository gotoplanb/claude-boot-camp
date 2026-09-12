# No Deep Link? Use the Web Share API to Reach an App's Documented Share-Sheet Entry Point

**Source:** Dave wanted a "read this in Eleven Reader" button on davestanton.com, Claude Boot Camp session 2026-09-12 — and was rightly sceptical a deep link existed.
**Type:** pattern
**Verified:** `ran-it` — shipped to davestanton.com and confirmed working end to end on a real iPhone: tapped **Listen** on a post in Safari, picked Eleven Reader from the share sheet, and the full article opened and played.
**Relevant to:** 4 (integrating), 2 (operating Claude products), general

## The problem

You want a button that sends your content into a mobile app. You go looking for a URL scheme (`someapp://…`) or a universal link, and there isn't one.

For Eleven Reader specifically, ElevenLabs' docs list exactly four ways in — type or paste text, scan with the camera, paste a link, upload a file. **No URL scheme, no universal link, no `?url=` web entry point.**

## The move

**Don't hunt for an undocumented URL pattern.** Even if you find one that works today, a button built on undocumented behaviour breaks silently when the vendor ships an update — and nothing tells you.

Instead, find the app's *documented* entry point and reach it with a standard. Most mobile apps register as a **share target**, which is documented, supported, and stable. `navigator.share()` opens the native share sheet from a button click with your URL pre-filled:

```html
<button id="listen-share" type="button" hidden>Listen</button>
<script>
(function () {
    var btn = document.getElementById('listen-share');
    if (!btn || !navigator.share) return;   // no dead button
    btn.hidden = false;
    btn.addEventListener('click', function () {
        navigator.share({ title: "…", url: window.location.href })
                 .catch(function () {});    // dismissal is an AbortError, not a failure
    });
})();
</script>
```

Cost: **one extra tap** versus a true deep link. Gain: documented behaviour on both sides, no dependency, no external request, nothing to break.

## Three things testing caught that assumptions missed

1. **Desktop browsers *do* have `navigator.share`.** The plan was "hide it on desktop, since the API won't exist there." Chrome on macOS reports `typeof navigator.share === "function"`, and Safari does too. The button shows there and opens the macOS sheet — AirDrop rather than a TTS app, still useful. Only Firefox desktop hides it. **The feature-detection worked; the prediction about what it would detect was wrong.**
2. **Dismissing the sheet rejects with `AbortError`.** Without a `.catch()`, every cancelled share logs an unhandled rejection.
3. **Escape the title properly.** It goes into a JS string literal, so a post title containing a quote breaks the script. Emit it through the template's JSON filter (`{{ title | tojson }}`), not string interpolation.

## The thing to actually verify

The receiving app **fetches your URL server-side** to convert it. If your site blocks bots — this one blocks AI crawlers at the Cloudflare edge — the app may get a challenge page instead of your content, and the failure looks like a working button producing gibberish audio.

Confirmed fine here, but it's the check to run before trusting it: tap the button on a real device and listen to the first ten seconds.

## Why this matters

The generalisable move is **look for the supported seam rather than the clever one**. "There's no API for this" often means "there's no *dedicated* API" — while a standard mechanism (share targets, file associations, the clipboard, a well-known file format) already reaches the same place with a guarantee behind it.

It's the same instinct as card 050: coupling to undocumented behaviour is coupling to a contract nobody agreed to.
