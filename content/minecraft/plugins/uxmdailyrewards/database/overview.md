---
title: Overview
order: 901
description: What is stored, the table, sharing one database across servers, keeping one server apart, and moving off SQLite.
icon: database
---

The database holds each player's progress: their streak and the moment of their last claim. Days, ranks and settings
are files and are not in it.

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. The default, a file with nothing to set up |
| [MySQL and MariaDB](mysql.md) | A network sharing each player's streak |
| [PostgreSQL](postgresql.md) | The same, on PostgreSQL |

The backend is set in the `storage` block of [config.conf](../config/config-conf.md#storage). It is opened once, so
a change needs a restart. The table is created and moved forward on its own at the start.

## The table

Every table name starts with `storage.table-prefix`, `uxmdailyrewards_` by default.

| Table | Holds |
|---|---|
| `uxmdailyrewards_progress` | One row per player who has claimed: `player_id`, `streak`, and `claimed_at` in milliseconds |
| `uxmlib_schema_history` | The version the table is at |

A player who never claimed, or whom an operator reset, has no row. `/uxmdailyrewards doctor` names the backend, where
the rows are, the prefix and the schema version.

## A network

Point every server at the same MySQL, MariaDB or PostgreSQL database with the same `table-prefix`, and a player's
streak follows them. A claim reads the database every time, so a player who claimed on one server is told
`claim.too-soon` on the next. Keep `claim`, `calendar` and the day files the same on every server, or a player's day
and reward depend on where they claim.

The placeholders and the reminder on one server see a claim made on another at the player's next login there.

## Keeping one server apart

`local-only = true` on one server keeps its rows in its own SQLite file, whatever `type` says, under the same prefix.
Use it for a server of the network that should run its own calendar, such as a test server. Keep the rest of the
`storage` block the same everywhere and set this on that one server alone.

## Moving off SQLite

1. Create the database and a user on the database server.
2. Set `type`, `host`, `port`, `database`, `username` and `password`, and `copy-from-file = true`.
3. Restart once. The rows of the SQLite file are copied into the empty table, and the console says how many.
4. Leave `copy-from-file` on or turn it off: a table that already holds rows is never copied into again, so nothing
   is copied twice.

The SQLite file is left where it is, as a backup.

## Erasing a player

`/uxmdailyrewards reset <player>` deletes the player's row, on every server sharing the database.

Related: [config.conf](../config/config-conf.md#storage), [SQLite](sqlite.md), [MySQL and MariaDB](mysql.md)
