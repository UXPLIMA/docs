---
title: seasons.conf
order: 641
description: "Seasons: length, boards and season rewards."
icon: calendar-range
---

Seasons: islands compete for a while, the season ends, the best are paid and a new one starts.

The file is `plugins/uxmSkyblock/modules/seasons.conf`. Every default below is the shipped value.

## `seasons`

| Key | Default | What it does |
|---|---|---|
| `season-number` | `1` | Active season number |
| `season-name` | `Season 1 - Genesis` | Season display name |
| `duration` | `30d` | Default duration for newly bootstrapped seasons (e.g. "30d", "14d", "60m") |
| `check-interval` | `60s` | Background verification interval to detect season expiration and trigger freezes |

## `seasons.rewards`

Competitive reward actions queued by island level rank placement Both legacy command strings and typed reward structures are supported: SQL_CURRENCY, EXTERNAL_VAULT, ITEM, PERMISSION, COSMETIC

| Key | Default | What it does |
|---|---|---|
| `1` | a list of 4 entries, below |  |
| `2` | a list of 2 entries, below |  |
| `3` | a list of 2 entries, below |  |

`seasons.rewards.1`:

| # | `type` | `currency` | `amount` | `item` | `permission` | `cosmetic-id` |
|---|---|---|---|---|---|---|
| 1 | `SQL_CURRENCY` | `PRIMARY` | `1000000` |  |  |  |
| 2 | `ITEM` |  | `16` | `NETHERITE_INGOT` |  |  |
| 3 | `PERMISSION` |  |  |  | `uxmskyblock.vip` |  |
| 4 | `COSMETIC` |  |  |  |  | `season_1_crown` |

`seasons.rewards.2`:

| # | `type` | `currency` | `amount` | `item` |
|---|---|---|---|---|
| 1 | `SQL_CURRENCY` | `PRIMARY` | `500000` |  |
| 2 | `ITEM` |  | `32` | `DIAMOND_BLOCK` |

`seasons.rewards.3`:

| # | `type` | `currency` | `amount` | `item` |
|---|---|---|---|---|
| 1 | `SQL_CURRENCY` | `PRIMARY` | `250000` |  |
| 2 | `ITEM` |  | `64` | `DIAMOND` |
