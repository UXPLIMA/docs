---
title: Reference
order: 701
description: Every %uxmparkour_...% placeholder, the leaderboard grammar, and what an empty place answers.
icon: braces
---

uxmParkour registers the `uxmparkour` expansion when PlaceholderAPI is installed. Every placeholder answers from
memory, so a hologram or a scoreboard that asks many times a second costs nothing.

## The player

| Placeholder | Renders |
|---|---|
| `%uxmparkour_current%` | The jumps of the run the player is in, `0` outside a run |
| `%uxmparkour_arena%` | The arena the player is running, empty outside a run |
| `%uxmparkour_best%` | The player's longest run ever |
| `%uxmparkour_total_jumps%` | Every jump the player has made |
| `%uxmparkour_total_runs%` | Every run the player has finished, runs of no jump included |
| `%uxmparkour_theme%` | The theme the player chose, empty when none |
| `%uxmparkour_difficulty%` | The difficulty the player chose, empty when none |
| `%uxmparkour_head%` | The player, for a `<head:...>` tag: their name, or their id when an account cannot have the name |

## Boards

Every UXM plugin with boards answers the same grammar, so a hologram written for one is copied for another by
changing the prefix.

| Placeholder | Renders |
|---|---|
| `%uxmparkour_top_<board>_<period>_<n>_name%` | The name at place `n`, or `placeholder.nobody` |
| `%uxmparkour_top_<board>_<period>_<n>_value%` | The score at place `n`, or `placeholder.none` |
| `%uxmparkour_top_<board>_<period>_<n>_head%` | The player at place `n` for a `<head:...>` tag, or `leaderboard.empty-head` |
| `%uxmparkour_rank_<board>_<period>%` | The reader's own place, or `placeholder.none` |
| `%uxmparkour_value_<board>_<period>%` | The reader's own score |
| `%uxmparkour_resets_<period>%` | The time until the period starts again, in the reader's words (`4d 1h`, `1h 30m`, `12m`) |

| `<board>` | Ranks |
|---|---|
| `best` | The longest run in the period |
| `jumps` | The jumps made in the period |
| `runs` | The runs of at least one jump in the period |

`<period>` is `alltime`, `monthly`, `weekly` or `daily`, from the periods kept in `leaderboard.periods`. `<n>` counts
from 1 up to `leaderboard.size`. A board, period or field the plugin does not have is left as written, so a typo
shows itself.

## A hologram

```
<h:'Parkour'>
#1 <head:%uxmparkour_top_best_alltime_1_head%> %uxmparkour_top_best_alltime_1_name% %uxmparkour_top_best_alltime_1_value%
#2 <head:%uxmparkour_top_best_alltime_2_head%> %uxmparkour_top_best_alltime_2_name% %uxmparkour_top_best_alltime_2_value%
#3 <head:%uxmparkour_top_best_alltime_3_head%> %uxmparkour_top_best_alltime_3_name% %uxmparkour_top_best_alltime_3_value%
You: %uxmparkour_rank_best_alltime% with %uxmparkour_value_best_alltime%
This week resets in %uxmparkour_resets_weekly%
```

The `<head:...>` tag needs a hologram plugin that draws it; uxmEssentials holograms do. An empty place draws
`leaderboard.empty-head`, Steve by default, so every line has a head.

## Notes

- **Fresh at the next read.** A finished run reaches the boards at the next `leaderboard.refresh-seconds`, 15 by
  default. The player placeholders are live.
- **Offline readers.** The `top` placeholders need no reader. `rank`, `value` and `head` answer for the player the
  line is drawn for.
- **The words.** `placeholder.nobody`, `placeholder.none` and `placeholder.resets.*` in the messages files, in the
  reader's language. Leave one empty to show nothing.
- **Without PlaceholderAPI** the placeholders answer nothing; `/parkour top` and `/parkour stats` still work.

Related: [config.conf](../config/config-conf.md#leaderboard), [Concepts](../getting-started/concepts.md#boards)
