---
title: Reference
order: 801
description: Every %uxmlevels_...% placeholder, the per track values, the board grammar, and what an empty place answers.
icon: braces
---

uxmLevels registers the `uxmlevels` expansion when PlaceholderAPI is installed. Every placeholder answers from memory.

## A player on a track

Written `%uxmlevels_<track>_<what>%`, such as `%uxmlevels_mining_level%`.

| `<what>` | Renders |
|---|---|
| `level` | The level held |
| `max_level` | The top of the ladder |
| `experience` | The experience on the track |
| `experience_formatted` | The same, grouped by thousands: `12,480` |
| `into` | Experience into the current level |
| `next` | Experience still needed for the next level |
| `percent` | How far through the level, a whole number |
| `prestige` | The prestige count |
| `badge` | The prestige badge as plain text; empty with no prestige |
| `badge_raw` | The badge as the MiniMessage written |
| `multiplier` | What a gain on the track is multiplied by, such as `1.25` |
| `position` | The place on the all time board; empty outside the snapshot |

A track that does not exist answers empty.

## Boards

Every UXM plugin with boards answers the same grammar. `<board>` is a track id.

| Placeholder | Renders |
|---|---|
| `%uxmlevels_top_<track>_<period>_<n>_name%` | The name at place `n`, or `placeholder.nobody` |
| `%uxmlevels_top_<track>_<period>_<n>_value%` | The score at place `n`: the level for `alltime`, experience earned for the others; or `placeholder.none` |
| `%uxmlevels_top_<track>_<period>_<n>_head%` | The player at place `n` for a `<head:...>` tag, or `leaderboard.empty-head` |
| `%uxmlevels_rank_<track>_<period>%` | The reader's own place, or `placeholder.none` |
| `%uxmlevels_value_<track>_<period>%` | The reader's own score; for `alltime`, their level |
| `%uxmlevels_resets_<period>%` | The time until the period starts again: `4d 1h`, `1h 30m`, `12m` |

`<period>` is `alltime`, `monthly`, `weekly` or `daily`, from `leaderboard.periods`. `<n>` counts from 1 up to
`leaderboard.size`. A key the plugin does not have is left as written.

## Examples

```
Mining %uxmlevels_mining_level% %uxmlevels_mining_badge%
#1 <head:%uxmlevels_top_global_weekly_1_head%> %uxmlevels_top_global_weekly_1_name% %uxmlevels_top_global_weekly_1_value%
This week resets in %uxmlevels_resets_weekly%
```

Related: [Boards](../tracks/boards.md), [PlaceholderAPI](../integrations/placeholderapi.md)
