---
title: PlaceholderAPI
order: 903
description: The uxmdailyrewards expansion, and placeholders from other plugins inside this one.
icon: braces
---

With PlaceholderAPI installed, uxmDailyRewards registers the `uxmdailyrewards` expansion at start, and takes it down
again when the plugin stops. Nothing needs downloading with `/papi ecloud`; the expansion is inside the plugin.

| You want | Use |
|---|---|
| The streak on a scoreboard | `%uxmdailyrewards_streak%` |
| A countdown to the next reward | `%uxmdailyrewards_time_left%` |
| A line only when a reward is waiting | `%uxmdailyrewards_available%` |

Every key is on the [reference](../placeholders/reference.md).

The other way round, a reward line's `{if=...}` condition may name any placeholder of another plugin, such as
`%player_level%`, and it is resolved through PlaceholderAPI.

Without PlaceholderAPI, the plugin runs fully and `/uxmdailyrewards doctor` warns that the placeholders publish
nothing.

Related: [Placeholders](../placeholders/reference.md)
