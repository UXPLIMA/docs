---
title: Overview
order: 901
description: What is stored, the tables, sharing one database across servers, keeping one server apart, and moving off SQLite.
icon: database
---

The database holds each player's experience and prestige per track, the rewards they took, their boost, the name they
last played under, and the period boards. Tracks, events and settings are files.

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. The default, a file with nothing to set up |
| [MySQL and MariaDB](mysql.md) | A network sharing levels and boards |
| [PostgreSQL](postgresql.md) | The same, on PostgreSQL |

The backend is set in the `storage` block of [config.conf](../config/config-conf.md#storage). It is opened once, so a
change needs a restart. The tables are created and moved forward on their own.

## The tables

Every table name starts with `storage.table-prefix`, `uxmlevels_` by default.

| Table | Holds |
|---|---|
| `uxmlevels_progress` | One row per player and track: `experience` since the last prestige and the `prestige` count |
| `uxmlevels_claim` | One row per reward a player took on the current climb |
| `uxmlevels_boost` | The running boost of a player: the multiplier and when it ends |
| `uxmlevels_name` | The name each player last played under, for the boards |
| `uxmlevels_board` | The period boards: one row per board, period, window and player |
| `uxmlevels_schema_history` | The version the tables are at |

The level is never stored; it is read from the experience through the track's curve.

## A network

Point every server at the same database with the same `table-prefix`, and a player's levels follow them.

- `sync-seconds` (2 by default) is how often a server reads what the others changed for the players who are on it: a
  level gained or a boost given elsewhere. `0` turns it off.
- Keep the track and event files the same on every server, or a level means different things in different places.
- Write the same `leaderboard.time-zone` and `week-starts` on every server.

## Keeping one server apart

`local-only = true` on one server keeps its rows in its own SQLite file, whatever `type` says, such as a creative or
build server whose levels should not count.

## Moving off SQLite

1. Create the database and a user on the database server.
2. Set `type`, `host`, `port`, `database`, `username` and `password`, and `copy-from-file = true`.
3. Restart once. The rows of the SQLite file are copied into the empty tables, and the console says how many.

The SQLite file is left where it is, as a backup.

## Erasing a player

`/uxmlevels reset <player>` sets every track back to the start and removes the player from every board, on every
server sharing the database.

Related: [config.conf](../config/config-conf.md#storage), [SQLite](sqlite.md), [MySQL and MariaDB](mysql.md)
