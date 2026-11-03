---
title: Reference
order: 801
description: "Every %uxmjobs_...% placeholder: per job, per player, and the boards."
icon: list
---

Registered when PlaceholderAPI is installed. Every answer comes out of memory; a request nothing knows answers nothing.

## Per job

Written `%uxmjobs_<fact>_<job>%`, such as `%uxmjobs_level_miner%`.

| Fact | Returns |
|---|---|
| `level` | The level, or 0 |
| `xp` | Experience in the job |
| `xp_next` | Experience left to the next level |
| `progress` | Percent through the level |
| `title` | The title of the level; empty when none is written |
| `joined` | `true` or `false` |
| `can_join` | Whether the player could join it now: not held, a free slot, a free seat, no rejoin cooldown. Requirements are not run |
| `workers` | How many hold it |
| `seats_left` | Free places; empty when there is no limit |

## Per player

| Placeholder | Returns |
|---|---|
| `%uxmjobs_points%` | The points balance |
| `%uxmjobs_slots%` | How many jobs the player holds |
| `%uxmjobs_slots_max%` | How many they may hold, read from their nodes |
| `%uxmjobs_prefix%` | The ids of the jobs they hold, separated by spaces |
| `%uxmjobs_zone%` | The zone they stand in; empty when none |
| `%uxmjobs_booster_<currency>%` | The combined booster for `money`, `experience` or `points` |
| `%uxmjobs_earned%`, `%uxmjobs_earned_<currency>%` | Earned in the income window (money by default) |
| `%uxmjobs_cap_left_<currency>%` | Left before the cap; empty with no cap |
| `%uxmjobs_cap_reset%` | Minutes until the income window starts again |
| `%uxmjobs_contract%` | The id of the live contract that ends soonest; empty when none |
| `%uxmjobs_contract_points%` | Contract points under it |
| `%uxmjobs_promotion_left%` | Contract points left to the promotion goal; empty when none runs |

## Boards

The shared board grammar. A board is a job id or `total`; a period is `alltime`, `monthly`, `weekly` or `daily`.

| Placeholder | Returns |
|---|---|
| `%uxmjobs_top_<board>_<period>_<n>_name%` | The player at place `n` |
| `%uxmjobs_top_<board>_<period>_<n>_value%` | Their score |
| `%uxmjobs_top_<board>_<period>_<n>_head%` | Their head, for a `<head:...>` tag; `leaderboard.empty-head` for an empty place |
| `%uxmjobs_rank_<board>_<period>%` | The player's own place |
| `%uxmjobs_value_<board>_<period>%` | The player's own score |
| `%uxmjobs_resets_<period>%` | Time until the period starts again, as `placeholder.resets` writes it |

All time ranks by level; a month, week or day by experience earned in it (levels on `total`). A job id may hold an
underscore: a key is read from its end, so `%uxmjobs_top_deep_miner_weekly_1_name%` is the `deep_miner` board. An empty
place reads `placeholder.nobody` ("Nobody") and its value `placeholder.none` ("-").

```
Miner %uxmjobs_level_miner% (%uxmjobs_progress_miner%%)
#1 this week: %uxmjobs_top_total_weekly_1_name%
```

Related: [Leaderboards](../features/leaderboards.md)
