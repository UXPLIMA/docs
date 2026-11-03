---
title: The log and housekeeping
order: 409
description: The opening log with the chance of every win, the log file, purging players who left, and trimming old rows.
icon: scroll-text
---

## The opening log

Every win is a row with the player, the crate, the reward, the time and **the chance that reward had at that
moment**. That column answers the accusation a paid crate attracts. A pick crate's rows are marked as a choice, so
nobody can claim it rolled.

| Key | Default | Meaning |
|---|---|---|
| `log.to-database` | `true` | Rows in the database, which `/uxmcrates log` reads |
| `log.to-file` | `true` | Lines in a file as well |
| `log.file` | `"logs/crates-%date%.log"` | The file, under `data/`. `%date%` starts a new file each day |
| `log.page-size` | `45` | Rows per page of the log window |

`/uxmcrates log player Steve`, `log crate elite` or `log reward enchanted-apple` reads the rows, newest first: in the
[log window](../menus/log.md) for a player, as lines for the console. Dates follow `date-format`.

## Purging players who left

Every table grows by one row per player who ever joined. The purge forgets the ones who stopped coming.

| Key | Default | Meaning |
|---|---|---|
| `purge.enabled` | `false` | Ships off |
| `purge.after-days` | `180` | How long a player may be away before their rows go. Never zero |
| `purge.on-start` | `true` | One pass when the server starts |
| `purge.every-hours` | `12` | A pass this often after that; `0` turns the timer off |

One pass takes at most 200 players, so a long-neglected server catches up over several passes instead of locking the
tables once. A player is judged by when they were last seen, written on every join and quit. **A player who is owed a
pending reward is never purged.**

## Trimming the log and the ledger

| Key | Default | Meaning |
|---|---|---|
| `retention.log-days` | `0` | Days of the opening log to keep |
| `retention.ledger-days` | `0` | Days of the key ledger to keep |

`0` keeps everything. A pass runs at start and on the purge's hour.

Related: [config.conf](../config/config-conf.md#log), [Database](../database/overview.md)
