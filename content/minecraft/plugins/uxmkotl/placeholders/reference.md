---
title: Reference
order: 801
description: Every %uxmkotl_...% placeholder, the king of an arena, the leaderboard grammar, and what an empty place answers.
icon: braces
---

uxmKotl registers the `uxmkotl` expansion when PlaceholderAPI is installed. Every placeholder answers from memory,
so a hologram or a scoreboard that asks many times a second costs nothing.

## An arena

These need no reader, so a hologram over a tower can show who holds it.

| Placeholder | Renders |
|---|---|
| `%uxmkotl_king_<arena>%` | The name of the king of that arena, or `placeholder.nobody` while the plate is free. Empty for an id that is not valid |
| `%uxmkotl_reign_<arena>%` | How long the king of that arena has held it, such as `1m 4s`; empty while the plate is free |

## The player

| Placeholder | Renders |
|---|---|
| `%uxmkotl_arena%` | The id of the arena the player is in, empty outside one |
| `%uxmkotl_reign%` | How long the player has held the crown this time; empty when they are not king |
| `%uxmkotl_held%` | The player's total time held, such as `12m 30s` |
| `%uxmkotl_longest%` | The player's longest single reign |
| `%uxmkotl_claims%` | Crowns the player has taken |
| `%uxmkotl_kills%` | Players the player has knocked off |
| `%uxmkotl_head%` | The player, for a `<head:...>` tag: their name, or their id when an account cannot have the name |

A time is written in days, hours, minutes and seconds, leaving out the units that are zero: `2h 5s`. No time at all
is `0s`.

## Boards

Every UXM plugin with boards answers the same grammar, so a hologram written for one is copied for another by
changing the prefix.

| Placeholder | Renders |
|---|---|
| `%uxmkotl_top_<board>_<period>_<n>_name%` | The name at place `n`, or `placeholder.nobody` |
| `%uxmkotl_top_<board>_<period>_<n>_value%` | The score at place `n`, or `placeholder.none` |
| `%uxmkotl_top_<board>_<period>_<n>_head%` | The player at place `n` for a `<head:...>` tag, or `leaderboard.empty-head` |
| `%uxmkotl_rank_<board>_<period>%` | The reader's own place, or `placeholder.none` |
| `%uxmkotl_value_<board>_<period>%` | The reader's own score |
| `%uxmkotl_resets_<period>%` | The time until the period starts again, in the reader's words (`4d 1h`, `1h 30m`, `12m`) |

| `<board>` | Ranks | Score shown as |
|---|---|---|
| `held` | Time held in the period, added up | a time |
| `longest` | The longest single reign in the period | a time |
| `claims` | Crowns taken in the period | a count |
| `kills` | Kills in the period | a count |

`<period>` is `alltime`, `monthly`, `weekly` or `daily`, from the periods kept in `leaderboard.periods`. `<n>` counts
from 1 up to `leaderboard.size`. A board, period or field the plugin does not have is left as written, so a typo
shows itself.

## A hologram

```
<h:'King of the Ladder'>
King: %uxmkotl_king_hill% %uxmkotl_reign_hill%
#1 <head:%uxmkotl_top_held_alltime_1_head%> %uxmkotl_top_held_alltime_1_name% %uxmkotl_top_held_alltime_1_value%
#2 <head:%uxmkotl_top_held_alltime_2_head%> %uxmkotl_top_held_alltime_2_name% %uxmkotl_top_held_alltime_2_value%
#3 <head:%uxmkotl_top_held_alltime_3_head%> %uxmkotl_top_held_alltime_3_name% %uxmkotl_top_held_alltime_3_value%
You: %uxmkotl_rank_held_alltime% with %uxmkotl_value_held_alltime%
This week resets in %uxmkotl_resets_weekly%
```

The `<head:...>` tag needs a hologram plugin that draws it; uxmEssentials holograms do. An empty place draws
`leaderboard.empty-head`, Steve by default, so every line has a head.

## Notes

- **Fresh at the next read.** A reign, a claim or a kill reaches the boards at the next `leaderboard.refresh-seconds`,
  15 by default. The arena and player placeholders are live.
- **Offline readers.** The `king`, `reign` and `top` placeholders need no reader. The player placeholders answer
  nothing without one.
- **The words.** `placeholder.nobody`, `placeholder.none` and `placeholder.resets.*` in the messages files, in the
  reader's language. Leave one empty to show nothing.
- **Without PlaceholderAPI** the placeholders answer nothing; `/kotl top` and `/kotl stats` still work.

Related: [config.conf](../config/config-conf.md#leaderboard), [Concepts](../getting-started/concepts.md#boards)
