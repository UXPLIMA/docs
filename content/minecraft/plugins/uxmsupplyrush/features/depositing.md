---
title: Depositing
order: 31
description: How players hand in items, and what keeps every deposit safe.
icon: package-plus
---

Players type `/sr`, open the running tournament, and hand in items from the event menu. The menu
shows every item the event accepts, with its point value and how many of it the player has already
deposited.

## Handing in items

| Action | Result |
|---|---|
| Click an item in the event menu | Opens the amount menu: 1, 16, 32, 64, or a custom amount typed in chat |
| Shift-click an item in the event menu | Deposits every one of that item the player carries |
| Click an item in their own inventory | Opens the amount menu for it |
| **Bulk Deposit Everything** | Deposits every accepted item the player carries, in one go |

Items come out of the player's inventory, never their armour slots or off hand. An item with a
custom name or lore only counts if it matches the event's item exactly.

## The event menu

- **Sort** switches between showing the most deposited items first and showing the items the player
  carries first.
- Items the player has already deposited glow, unless `deposited-deliverables-glow` is off in
  [general-settings.yml](../config/general-settings.md).
- **Leaderboard** opens the current standings, and **Rewards** shows the event's
  `rewards-explanation` lines.

A deposit plays the `deposit` sound, and a bulk deposit the `bulk-deposit` sound, both set in
`general-settings.yml`.

## When a deposit is refused

| Situation | What happens |
|---|---|
| No event is running, or it is paused | Nothing is taken |
| The run's time is up | Nothing is taken, even if the end is still being announced |
| The player is in creative mode | Refused while `creative-deposit` is `DENY`, unless they hold `uxmsupplyrush.bypass.creative-deposit` |
| The item does not count for this event | Nothing is taken, and the player is told |

## Deposits are never lost

The plugin saves a deposit before it tells the player it went through. If it cannot be saved, the
items are given back; if the player has already left, they get them back the next time they join.
A deposit cut off by a server crash is settled on the next start, so it is neither lost nor counted
twice.

Related: [Events and runs](events-and-runs.md), [Ranking](ranking.md)
