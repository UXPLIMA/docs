---
title: Reference
order: 801
description: Every %uxmskills_...% placeholder and its contract.
icon: braces
---

Every value is plain text: no colour, no unit, no thousands separator and never a percent sign. A count is a whole
number and a rate carries exactly two decimals. A key that exists but has no value right now answers `-`; a genuine
zero is `0`; an unknown key is left as written. Values come off a snapshot rebuilt every `placeholders.refresh-ms`.

## A skill

| Placeholder | Gives |
|---|---|
| `%uxmskills_<skill>_level%` | The level |
| `%uxmskills_<skill>_experience%` | The total experience |
| `%uxmskills_<skill>_maximum%` | The cap |
| `%uxmskills_<skill>_into%` | Experience into this level |
| `%uxmskills_<skill>_next%` | Experience to the next |
| `%uxmskills_<skill>_cost%` | What this level costs |
| `%uxmskills_<skill>_percent%` | Progress through it |
| `%uxmskills_<skill>_bar%` | The same as a bar, drawn by `progress-bar` |
| `%uxmskills_<skill>_multiplier%` | The multiplier on it |
| `%uxmskills_<skill>_badge%` | The capstone badge, once earned |
| `%uxmskills_<skill>_position%` | The all time board place |

## The player

| Placeholder | Gives |
|---|---|
| `%uxmskills_total_level%` | Every level summed |
| `%uxmskills_rate%` | The anti-farm rate, out of 100 |
| `%uxmskills_rate_reason%` | The term pulling it down most |
| `%uxmskills_ability_<id>%` | An ability's value for the player |
| `%uxmskills_stat_<id>%`, `%uxmskills_stat_<id>_points%` | A stat's value and points |
| `%uxmskills_trait_<key>%` | A trait's value |
| `%uxmskills_resource_<id>%`, `_maximum%`, `_percent%`, `_bar%` | Mana, stamina and the like |

## Boards

| Placeholder | Gives |
|---|---|
| `%uxmskills_top_<board>_<period>_<n>_name%`, `_value%`, `_head%` | Place `n` |
| `%uxmskills_rank_<board>_<period>%` | Your place |
| `%uxmskills_value_<board>_<period>%` | Your value |
| `%uxmskills_resets_<period>%` | When the period resets |

`<board>` is a skill id, `total` or `average`; `<period>` is `alltime`, `monthly`, `weekly` or `daily`. A place nobody
holds still answers: the word for nobody, the word for none, and `leaderboard.empty-head`.

Related: [Leaderboards](../features/leaderboards.md)
