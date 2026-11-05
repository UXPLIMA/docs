---
title: Reference
order: 751
description: Every placeholder, what it renders, and whether it answers for an offline player.
icon: braces
---

PlaceholderAPI, identifier `uxmskyblock`. The plugin needs PlaceholderAPI installed for these.

## The reader's island

Each answers for the player it is asked about, online or not. Values are kept for a short while and read again in the
background: the first request for a player not read yet answers empty (or `false`, `0`) and starts the read.

| Placeholder | Value |
|---|---|
| `%uxmskyblock_has_island%` | `true` when the player belongs to an island |
| `%uxmskyblock_island_id%` | the island's id |
| `%uxmskyblock_island_role%`, `%uxmskyblock_role%` | the player's role on the island |
| `%uxmskyblock_island_members_count%`, `%uxmskyblock_members_count%` | how many members the island has |
| `%uxmskyblock_island_bank_balance%`, `%uxmskyblock_bank_balance%` | the bank balance, formatted |
| `%uxmskyblock_island_bank_balance_raw%`, `%uxmskyblock_bank_balance_raw%` | the bank balance in minor units |
| `%uxmskyblock_island_bank_crystals%`, `%uxmskyblock_bank_crystals%` | the island's crystals |
| `%uxmskyblock_island_upgrade_tier_<upgrade>%`, `%uxmskyblock_upgrade_tier_<upgrade>%` | the tier the island holds of an upgrade |
| `%uxmskyblock_oneblock_is_oneblock%` | `true` on a OneBlock island |
| `%uxmskyblock_oneblock_<value>%` | `phase`, `phase_id`, `phase_number`, `phase_count`, `blocks_broken`, `phase_blocks`, `phase_length`, `phase_left`, `phase_percent`, `next_phase`, `next_phase_id`, `next_moment`, `cycle`, `my_breaks`, `top_member`, `top_member_breaks` |
| `%uxmskyblock_chunkblock_is_chunkblock%` | `true` on a ChunkBlock island |
| `%uxmskyblock_chunkblock_open_chunks%`, `%uxmskyblock_chunkblock_next_level%` | the open chunks and the next level of a ChunkBlock island |

`<upgrade>` is an upgrade's key, such as `island_size` or `member_limit`; see [Upgrades](../features/upgrades.md). The
bank balance is written with two decimals, such as `1500.00`.

## Leaderboards

Three boards, `level`, `worth` and `bank`, each for all time (`alltime`). A place is an island, under its owner's name
and head; a player's own place and value are their island's. These answer for anyone, online or not.

| Placeholder | Value |
|---|---|
| `%uxmskyblock_top_<board>_alltime_<n>_name%` | the owner of the island at place `n` |
| `%uxmskyblock_top_<board>_alltime_<n>_value%` | that island's level, worth or bank balance |
| `%uxmskyblock_top_<board>_alltime_<n>_head%` | the owner's name or id, for a `<head:...>` tag |
| `%uxmskyblock_rank_<board>_alltime%` | the reader's island's place |
| `%uxmskyblock_value_<board>_alltime%` | the reader's island's value |

A place nobody holds answers the language files' `placeholder.nobody` (Nobody), and a missing value
`placeholder.none` (`-`). `n` runs from 1 to `leaderboard.size` in [config.conf](../config/config-conf.md), 10 as shipped.
