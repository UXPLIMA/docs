---
title: Reference
order: 601
description: Every %uxmhubpvp_...% placeholder, the leaderboard grammar, and what an empty place answers.
icon: braces
---

uxmHubPvP registers the `uxmhubpvp` expansion when PlaceholderAPI is installed. Every placeholder answers from
memory, so a hologram or a scoreboard that asks many times a second costs nothing.

## The player

| Placeholder | Renders |
|---|---|
| `%uxmhubpvp_in_pvp%` | `true` while the player is fighting, or warming up to leave; `false` otherwise |
| `%uxmhubpvp_tagged%` | `true` while the player is combat tagged |
| `%uxmhubpvp_kills%` | The player's kills |
| `%uxmhubpvp_deaths%` | The player's deaths |
| `%uxmhubpvp_ratio%` | Kills divided by deaths, two decimals: `2.47` |
| `%uxmhubpvp_streak%` | Kills since the player last died |
| `%uxmhubpvp_best_streak%` | The player's best streak |
| `%uxmhubpvp_head%` | The player, for a `<head:...>` tag: their name, or their id when an account cannot have the name |

`in_pvp` and `tagged` are what another plugin reads to know a player is busy, such as a scoreboard that changes
while a fight runs.

## Boards

Every UXM plugin with boards answers the same grammar, so a hologram written for one is copied for another by
changing the prefix.

| Placeholder | Renders |
|---|---|
| `%uxmhubpvp_top_<board>_<period>_<n>_name%` | The name at place `n`, or `placeholder.nobody` |
| `%uxmhubpvp_top_<board>_<period>_<n>_value%` | The score at place `n`, or `placeholder.none` |
| `%uxmhubpvp_top_<board>_<period>_<n>_head%` | The player at place `n` for a `<head:...>` tag, or `leaderboard.empty-head` |
| `%uxmhubpvp_rank_<board>_<period>%` | The reader's own place, or `placeholder.none` |
| `%uxmhubpvp_value_<board>_<period>%` | The reader's own score |
| `%uxmhubpvp_resets_<period>%` | The time until the period starts again, in the reader's words (`4d 1h`, `1h 30m`, `12m`) |

| `<board>` | Ranks |
|---|---|
| `kills` | Kills in the period |
| `streak` | The longest streak reached in the period |

`<period>` is `alltime`, `monthly`, `weekly` or `daily`, from the periods kept in `leaderboard.periods`. `<n>` counts
from 1 up to `leaderboard.size`. A board, period or field the plugin does not have is left as written, so a typo
shows itself.

## A hologram

```
<h:'Lobby PvP'>
#1 <head:%uxmhubpvp_top_kills_weekly_1_head%> %uxmhubpvp_top_kills_weekly_1_name% %uxmhubpvp_top_kills_weekly_1_value%
#2 <head:%uxmhubpvp_top_kills_weekly_2_head%> %uxmhubpvp_top_kills_weekly_2_name% %uxmhubpvp_top_kills_weekly_2_value%
#3 <head:%uxmhubpvp_top_kills_weekly_3_head%> %uxmhubpvp_top_kills_weekly_3_name% %uxmhubpvp_top_kills_weekly_3_value%
You: %uxmhubpvp_rank_kills_weekly% with %uxmhubpvp_value_kills_weekly%
Best streak this week: %uxmhubpvp_top_streak_weekly_1_name% (%uxmhubpvp_top_streak_weekly_1_value%)
Resets in %uxmhubpvp_resets_weekly%
```

The `<head:...>` tag needs a hologram plugin that draws it; uxmEssentials holograms do.

## Notes

- **Fresh at the next read.** A kill reaches the boards at the next `leaderboard.refresh-seconds`, 15 by default. The
  player placeholders are live.
- **Offline readers.** The `top` placeholders need no reader. The player placeholders answer for the player the line
  is drawn for.
- **The words.** `placeholder.nobody`, `placeholder.none` and `placeholder.resets.*` in the messages files, in the
  reader's language. Leave one empty to show nothing.
- **Without PlaceholderAPI** the placeholders answer nothing; `/pvp stats` and `/pvp top` still work.

Related: [config.conf](../config/config-conf.md#leaderboard), [Concepts](../getting-started/concepts.md#boards)
