---
title: PlaceholderAPI
order: 903
description: The uxmeastereggs expansion, and placeholders from other plugins inside this one.
icon: braces
---

With PlaceholderAPI installed, uxmEasterEggs registers the `uxmeastereggs` expansion at start. Nothing needs downloading with
`/papi ecloud`; the expansion is inside the plugin.

| You want | Use |
|---|---|
| A player's progress on a scoreboard | `%uxmeastereggs_found%` of `%uxmeastereggs_total%`, `%uxmeastereggs_percent%` |
| A line that changes once a player has them all | `%uxmeastereggs_complete%` |
| A leaderboard on a hologram | `%uxmeastereggs_top_found_alltime_1_name%` and the rest of the [board keys](../placeholders/reference.md#boards) |
| A reset timer | `%uxmeastereggs_resets_weekly%` |

Every key is on the [reference](../placeholders/reference.md).

The other way round, a moment's `{if=...}` condition may name any placeholder of another plugin, such as
`%player_level%`, and it is resolved through PlaceholderAPI.

Without PlaceholderAPI, the plugin runs fully and `/uxmeastereggs doctor` warns that the placeholders publish nothing.

Related: [Placeholders](../placeholders/reference.md)
