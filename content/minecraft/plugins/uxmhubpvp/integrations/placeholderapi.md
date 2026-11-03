---
title: PlaceholderAPI
order: 804
description: The uxmhubpvp expansion, and placeholders from other plugins inside this one.
icon: braces
---

With PlaceholderAPI installed, uxmHubPvP registers the `uxmhubpvp` expansion at start. Nothing needs downloading with
`/papi ecloud`; the expansion is inside the plugin.

| You want | Use |
|---|---|
| A fighter's record on a scoreboard | `%uxmhubpvp_kills%`, `%uxmhubpvp_deaths%`, `%uxmhubpvp_ratio%`, `%uxmhubpvp_streak%` |
| A line that changes while a fight runs | `%uxmhubpvp_in_pvp%`, `%uxmhubpvp_tagged%` |
| A leaderboard on a hologram | `%uxmhubpvp_top_kills_alltime_1_name%` and the rest of the [board keys](../placeholders/reference.md#boards) |
| A reset timer | `%uxmhubpvp_resets_weekly%` |

Every key is on the [reference](../placeholders/reference.md).

The other way round, a moment's `{if=...}` condition may name any placeholder of another plugin, such as
`%player_level%`, and the [`hands-off-when`](other-games.md) guards are placeholders of other plugins too.

Without PlaceholderAPI, the plugin runs fully and `/uxmhubpvp doctor` warns that the placeholders publish nothing.

Related: [Placeholders](../placeholders/reference.md)
