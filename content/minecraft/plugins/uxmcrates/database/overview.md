---
title: Overview
order: 901
description: What is stored, the tables, sharing one database across servers, keeping one server apart, and moving off SQLite.
icon: database
---

The database holds what a player owns and was promised: key balances, the key ledger, pending rewards, openings,
win counts, cooldowns, rerolls, milestones, the log, the boards, names and last-seen times. Crates, keys and rewards
are files.

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. The default |
| [MySQL and MariaDB](mysql.md) | A network sharing keys, limits and boards |
| [PostgreSQL](postgresql.md) | The same, on PostgreSQL |

## Tables

Every table name starts with `storage.table-prefix`, `uxmcrates_` by default.

| Table | Holds |
|---|---|
| `uxmcrates_key_balance` | One row per player and key: the virtual balance |
| `uxmcrates_key_ledger` | Every key grant, spend, transfer and set |
| `uxmcrates_pending` | One row per reward decided and not yet delivered |
| `uxmcrates_pending_item`, `uxmcrates_pending_command`, `uxmcrates_pending_money` | What each pending reward gives |
| `uxmcrates_opening_count` | Openings per player and crate |
| `uxmcrates_player_win`, `uxmcrates_global_win` | Win counts for limits, per player and for the server |
| `uxmcrates_cooldown` | The last opening per player and crate |
| `uxmcrates_reroll` | Granted rerolls |
| `uxmcrates_milestone`, `uxmcrates_milestone_all` | Rungs taken, per crate and across every crate |
| `uxmcrates_log` | The opening log, with the chance of each win |
| `uxmcrates_board` | The period boards of crates opened |
| `uxmcrates_name` | The name each player last played under, for the boards |
| `uxmcrates_seen` | When each player was last seen, for the purge |
| `uxmcrates_schema_history` | The version the tables are at |

## A network

Point every server at the same database with the same `table-prefix`, and a player's keys, pending rewards, limits,
cooldowns and milestones follow them. A global limit, such as three enchanted apples across the server, is then
across the network.

- Keep the crate, key and reward files the same on every server.
- Write the same `leaderboard.time-zone` and `week-starts` on every server.

## Keeping one server apart

`local-only = true` on one server keeps its rows in its own SQLite file, whatever `type` says, under the same prefix:
a creative or build server whose keys should not count. Keep the storage block otherwise the same on every server.

## Moving off SQLite

1. Create the database and a user on the database server.
2. Set `type`, `host`, `port`, `database`, `username` and `password`, and `copy-from-file = true`.
3. Restart once. The rows of the SQLite file are copied into the empty tables, and the console says how many.

Tables that already hold rows are left alone, so leaving `copy-from-file` on copies nothing twice. The SQLite file
stays where it is, as a backup.

## Growth

The purge forgets players who stopped coming, and retention trims the log and the ledger. Both ship off. See
[The log and housekeeping](../features/log-and-housekeeping.md).

Related: [config.conf](../config/config-conf.md#storage), [SQLite](sqlite.md), [MySQL and MariaDB](mysql.md)
