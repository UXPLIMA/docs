---
title: Overview
order: 1001
description: What is stored, the tables, sharing one database across servers, keeping one server apart, and moving off SQLite.
icon: database
---

The database holds what a player has done: their record, their choices and their place on every board. Arenas and
settings are files and are not in it.

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. The default, a file with nothing to set up |
| [MySQL and MariaDB](mysql.md) | A network sharing records and boards |
| [PostgreSQL](postgresql.md) | The same, on PostgreSQL |

The backend is set in the `storage` block of [config.conf](../config/config-conf.md#storage). It is opened once, so
a change needs a restart. The tables are created and moved forward on their own at the start.

## The tables

Every table name starts with `storage.table-prefix`, `uxmparkour_` by default.

| Table | Holds |
|---|---|
| `uxmparkour_profile` | One row per player: best run, total jumps, total runs, chosen theme, chosen difficulty |
| `uxmparkour_board` | One row per player, board, period and window: the score and when it was reached |
| `uxmparkour_name` | The name each player last ran under, for the boards and `reset` |
| `uxmparkour_schema_history` | The version the tables are at |

`/uxmparkour doctor` names the backend, where the rows are, the prefix and the schema version.

## A network

Point every server at the same MySQL, MariaDB or PostgreSQL database with the same `table-prefix`, and the servers
share each player's record, choices and board standings. A player who runs on one server reads the same best on
another.

- `sync-seconds` (2 by default) is how often a server reads what the others changed for the players who are on it.
  `0` turns it off.
- The boards are read from the database every `leaderboard.refresh-seconds`, so every server shows the same board.
- Write the same `leaderboard.time-zone` and `week-starts` on every server, or their weeks end at different times.
- Copy `arenas.conf` to each server that runs the same arenas.

## Keeping one server apart

`local-only = true` on one server keeps its rows in its own SQLite file, whatever `type` says, under the same prefix.
Use it for a server of the network that should not touch the shared records, such as a build or test server. Keep
the rest of the `storage` block the same everywhere and set this on that one server alone.

## Moving off SQLite

1. Create the database and a user on the database server.
2. Set `type`, `host`, `port`, `database`, `username` and `password`, and `copy-from-file = true`.
3. Restart once. The rows of the SQLite file are copied into the empty tables, and the console says how many.
4. Leave `copy-from-file` on or turn it off: tables that already hold rows are never copied into again, so nothing
   is copied twice.

The SQLite file is left where it is, as a backup.

## Erasing a player

`/uxmparkour reset <player>` sets the player's best, total jumps and total runs to zero and deletes every board row
they hold, in every period and window, on every server sharing the database. Their theme and difficulty stay.

Related: [config.conf](../config/config-conf.md#storage), [SQLite](sqlite.md), [MySQL and MariaDB](mysql.md)
