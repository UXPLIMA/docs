---
title: Passes and pass
order: 503
description: The pass list and one pass's ladder, with tiers, standing and claiming.
icon: ticket
---

## Passes

Passes in slots 10 to 16 (`quests:passes`), each an enchanted book showing tier, experience and track; a click opens its
[ladder](#pass) (`quests:pass-open`). Paging at 18 and 26, and the feather at 22 goes back to the quests window
(`quests:back`). It is opened by the star of the quests window.

## Pass

Six rows. Tiers in slots 0 to 35 (`quests:tiers`); a click on any tier takes every reward reached
(`quests:pass-claim`).

| Slot | Item |
|---|---|
| 36 | Back to the passes (`quests:pass-back`) |
| 37, 43 | Paging |
| 40 | Your standing: tier, experience, booster |
| 44 | Claim everything reached |

| Token | Means |
|---|---|
| `%pass%`, `%pass-name%` | The pass id and its name |
| `%tier%`, `%experience%`, `%track%` | Where the reader stands |
| `%free%`, `%premium%` | What the reader can do about each track of a tier |
| `%tier-state%` | `claimed`, `ready` or `locked`, in the reader's language |
| `%tier-material%` | `GRAY_DYE` claimed, `GOLD_INGOT` ready, `IRON_NUGGET` locked |
| `%booster%` | The booster the reader has |

The condition `quests:tier-state` gates an item on claimed, ready or locked.

Related: [Battle pass](../features/battle-pass.md)
