# Licensing a Knowledge Corpus — ShareAlike Beats NonCommercial When Your Audience Is Paid Consultants

**Source:** Claude Boot Camp licensing decision, 2026-09-11. Dave's requirement: "use verbatim or make derivatives, but they can't bundle them up and sell them, and if they distribute them elsewhere they need to link back." His first instinct was GPLv2.
**Type:** decision
**Verified:** n/a — a project decision. The license characterisations below are from the license texts themselves, not legal advice. `[confirm-this: a lawyer has not reviewed this.]`
**Relevant to:** general

## Content

The chosen license is **CC BY-SA 4.0** for the whole corpus.

### Why not GPL

GPL is the wrong family twice over. It's built for source code, not prose — and more importantly, **GPL does not prohibit selling.** You may charge anything you like for GPL software. What it requires is that recipients get the source and the same rights.

That's not a ban on commerce; it's a mechanism that makes *reselling* commercially pointless, because your buyer can legally redistribute for free. The instinct behind reaching for GPL was right — copyleft — but the tool was wrong.

### Why not NonCommercial

`CC BY-NC-SA` is the literal reading of "they can't sell it," and it was rejected for a specific reason: **the audience for this material is consultants and systems integrators.** NC forbids use "primarily intended for or directed toward commercial advantage or monetary compensation," and it is famously ambiguous about exactly this case — a consultant reading a card while doing paid client work.

Licensing so that your intended readers can't confidently use the material at work defeats the purpose. NC also isn't considered a free-culture license, and some organisations forbid NC-licensed content in internal material — another way it would block the exact reader it's written for.

### Why ShareAlike

`CC BY-SA` gets the desired outcome without the collateral damage:

| Requirement | How BY-SA delivers |
|---|---|
| Use verbatim | Permitted outright |
| Make derivatives | Permitted outright |
| Credit + link back | The `BY` clause requires attribution and an indication of changes |
| Can't bundle and sell | Not forbidden — made worthless. Any bundle must also be BY-SA, so buyers can legally give it away |
| Consultant uses it at work | Explicitly fine, no ambiguity |

## Why this matters

The general lesson transfers to any knowledge corpus: **decide whether you want to forbid commerce or merely remove the incentive to repackage.** They sound the same and they are not.

Forbidding it (NC) is blunt, catches legitimate professional use, and creates uncertainty precisely where you want confident adoption. Removing the incentive (SA) leaves the material freely usable by everyone — including commercially — while ensuring anything built on it stays equally open.

Worth noting the failure mode this avoids: a public repo with **no LICENSE file at all** is *all rights reserved* by default. Anyone who followed the "grab a few cards and drop them in a Project" pitch would technically have been infringing. Publishing an invitation to reuse without a license that permits it is a real, common mistake.
