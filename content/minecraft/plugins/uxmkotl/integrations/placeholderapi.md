---
title: PlaceholderAPI
order: 903
description: The uxmkotl expansion, and placeholders from other plugins inside this one.
icon: braces
---

With PlaceholderAPI installed, uxmKotl registers the `uxmkotl` expansion at start. Nothing needs downloading with
`/papi ecloud`; the expansion is inside the plugin.

| You want | Use |
|---|---|
| The king of an arena over its tower | `%uxmkotl_king_<arena>%`, `%uxmkotl_reign_<arena>%` |
| A player's record on a scoreboard | `%uxmkotl_held%`, `%uxmkotl_claims%`, `%uxmkotl_kills%` |
| A leaderboard on a hologram | `%uxmkotl_top_held_alltime_1_name%` and the rest of the [board keys](../placeholders/reference.md#boards) |
| A reset timer | `%uxmkotl_resets_weekly%` |

Every key is on the [reference](../placeholders/reference.md).

The other way round, a moment's `{if=...}` condition may name any placeholder of another plugin, such as
`%player_level%`, and it is resolved through PlaceholderAPI.

Without PlaceholderAPI, the plugin runs fully and `/uxmkotl doctor` warns that the placeholders publish nothing.

Related: [Placeholders](../placeholders/reference.md)
