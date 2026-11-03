---
title: PlaceholderAPI
order: 803
description: The uxmparkour expansion, and placeholders from other plugins inside this one.
icon: braces
---

With PlaceholderAPI installed, uxmParkour registers the `uxmparkour` expansion at start. Nothing needs downloading
with `/papi ecloud`; the expansion is inside the plugin.

| You want | Use |
|---|---|
| A player's best or current run on a scoreboard | `%uxmparkour_best%`, `%uxmparkour_current%` |
| A leaderboard on a hologram | `%uxmparkour_top_best_alltime_1_name%` and the rest of the [board keys](../placeholders/reference.md#boards) |
| A reset timer | `%uxmparkour_resets_weekly%` |

Every key is on the [reference](../placeholders/reference.md).

The other way round, a moment's `{if=...}` condition may name any placeholder of another plugin, such as
`%player_level%`, and it is resolved through PlaceholderAPI.

Without PlaceholderAPI, the plugin runs fully and `/uxmparkour doctor` warns that the placeholders publish nothing.

Related: [Placeholders](../placeholders/reference.md)
