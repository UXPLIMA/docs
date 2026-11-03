---
title: Reference
order: 801
description: Every %uxmeastereggs_...% placeholder, the leaderboard grammar, and what an empty place answers.
icon: braces
---

uxmEasterEggs registers the `uxmeastereggs` expansion when PlaceholderAPI is installed. Every placeholder answers from
memory, so a hologram or a scoreboard that asks many times a second costs nothing.

## The player

| Placeholder | Renders |
|---|---|
| `%uxmeastereggs_found%` | The eggs the player has found, of those on the map |
| `%uxmeastereggs_total%` | The eggs on the map |
| `%uxmeastereggs_percent%` | Found out of total, in whole per cent, rounded down; `0` on an empty map |
| `%uxmeastereggs_complete%` | `true` when the player has found every egg, `false` otherwise or on an empty map |
| `%uxmeastereggs_head%` | The player, for a `<head:...>` tag: their name, or their id when an account cannot have the name |

## Boards

Every UXM plugin with boards answers the same grammar, so a hologram written for one is copied for another by
changing the prefix.

| Placeholder | Renders |
|---|---|
| `%uxmeastereggs_top_<board>_<period>_<n>_name%` | The name at place `n`, or `placeholder.nobody` |
| `%uxmeastereggs_top_<board>_<period>_<n>_value%` | The score at place `n`, or `placeholder.none` |
| `%uxmeastereggs_top_<board>_<period>_<n>_head%` | The player at place `n` for a `<head:...>` tag, or `leaderboard.empty-head` |
| `%uxmeastereggs_rank_<board>_<period>%` | The reader's own place, or `placeholder.none` |
| `%uxmeastereggs_value_<board>_<period>%` | The reader's own score |
| `%uxmeastereggs_resets_<period>%` | The time until the period starts again, in the reader's words (`4d 1h`, `1h 30m`, `12m`) |

| `<board>` | Ranks |
|---|---|
| `found` | First finds in the period. A repeat find of a repeatable egg moves no board |

`<period>` is `alltime`, `monthly`, `weekly` or `daily`, from the periods kept in `leaderboard.periods`. `<n>` counts
from 1 up to `leaderboard.size`. A board, period or field the plugin does not have is left as written, so a typo
shows itself.

## A hologram

```
<h:'Egg Hunt'>
#1 <head:%uxmeastereggs_top_found_alltime_1_head%> %uxmeastereggs_top_found_alltime_1_name% %uxmeastereggs_top_found_alltime_1_value%
#2 <head:%uxmeastereggs_top_found_alltime_2_head%> %uxmeastereggs_top_found_alltime_2_name% %uxmeastereggs_top_found_alltime_2_value%
#3 <head:%uxmeastereggs_top_found_alltime_3_head%> %uxmeastereggs_top_found_alltime_3_name% %uxmeastereggs_top_found_alltime_3_value%
You: %uxmeastereggs_found% of %uxmeastereggs_total% (%uxmeastereggs_percent%%)
This week's best: %uxmeastereggs_top_found_weekly_1_name%
```

The `<head:...>` tag needs a hologram plugin that draws it; uxmEssentials holograms do.

## Notes

- **Fresh at the next read.** A find reaches the boards at the next `leaderboard.refresh-seconds`, 15 by default. The
  player placeholders are live.
- **Offline readers.** The `top` placeholders need no reader. The player placeholders answer for the player the line
  is drawn for.
- **The words.** `placeholder.nobody`, `placeholder.none` and `placeholder.resets.*` in the messages files, in the
  reader's language. Leave one empty to show nothing.
- **Without PlaceholderAPI** the placeholders answer nothing; the menus and `/eggs top` still work.

Related: [config.conf](../config/config-conf.md#leaderboard), [Concepts](../getting-started/concepts.md#the-board)
