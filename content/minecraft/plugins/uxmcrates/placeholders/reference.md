---
title: Reference
order: 801
description: Every %uxmcrates_...% placeholder, what it answers, and the board grammar.
icon: list
---

PlaceholderAPI is required; the identifier is `uxmcrates`. A placeholder is asked per player and answered from a copy
held in memory, read again every `placeholders.refresh-seconds` (5) per online player, off the server thread. A
scoreboard asks every tick and never reaches the database.

`<crate>`, `<key>` and `<reward>` are file ids, such as `common`, `common-key` and `diamond`.

## Keys

| Placeholder | Answer |
|---|---|
| `%uxmcrates_keys_<crate>%` | The keys that crate takes, held by the player: items and balance |
| `%uxmcrates_keys_total%` | Every key the player holds |
| `%uxmcrates_virtual_keys_<key>%` | One key's balance, without items |

## Openings and waits

| Placeholder | Answer |
|---|---|
| `%uxmcrates_opened_<crate>%` | How many times the player opened the crate |
| `%uxmcrates_opened_total%` | How many times the player opened anything |
| `%uxmcrates_cooldown_<crate>%` | The wait left, such as `6h 12m` |
| `%uxmcrates_cooldown_seconds_<crate>%` | The same wait in whole seconds |
| `%uxmcrates_pending%` | How many rewards are waiting |
| `%uxmcrates_rerolls%` | The rerolls the player was granted |
| `%uxmcrates_milestone_<crate>%` | Openings left to that crate's next rung |
| `%uxmcrates_milestone%` | Openings left to the next every-crate rung |

## Crates

| Placeholder | Answer |
|---|---|
| `%uxmcrates_last_winner_<crate>%` | Who last won from the crate |
| `%uxmcrates_last_reward_<crate>%` | What they won |
| `%uxmcrates_chance_<crate>_<reward>%` | The player's real chance of the reward as a percentage, with `odds.decimals` places. Empty while `odds.show` is off |

## The board of crates opened

The board answers the grammar every UXM plugin's boards share, so a hologram written for another plugin's board works
here. The board is `opened`; `<period>` is `alltime`, `monthly`, `weekly` or `daily`. Every opening counts, one at a
time or in bulk; the all time board starts from the openings counted before it existed.

| Placeholder | Answer |
|---|---|
| `%uxmcrates_top_opened_<period>_<n>_name%` | The name at place n, or `Nobody` |
| `%uxmcrates_top_opened_<period>_<n>_value%` | How many crates the player at place n opened, or `-` |
| `%uxmcrates_top_opened_<period>_<n>_head%` | The player at place n for a `<head:...>` tag, or `leaderboard.empty-head` |
| `%uxmcrates_rank_opened_<period>%` | The reader's own place, or `-` |
| `%uxmcrates_value_opened_<period>%` | How many crates the reader opened in the period |
| `%uxmcrates_resets_<period>%` | The time until the period starts again, such as `4d 12h`, or `-` for all time |
| `%uxmcrates_head%` | The reader's own head, for a `<head:...>` tag |

The places are read every `leaderboard.refresh-seconds` (15). `leaderboard` in
[config.conf](../config/config-conf.md#leaderboard) also sets the size (10), the periods, the time zone, the first day
of the week and how many past windows are kept. The words `Nobody` and `-` and the reset spans are `placeholder.*` in
the messages files.

## Examples

```
Common keys: %uxmcrates_keys_common%
Elite in: %uxmcrates_cooldown_elite%
#1 this week: %uxmcrates_top_opened_weekly_1_name% (%uxmcrates_top_opened_weekly_1_value%)
```

An unknown placeholder is left as written, which tells an operator they wrote one the plugin does not have.

Related: [PlaceholderAPI](../integrations/placeholderapi.md)
