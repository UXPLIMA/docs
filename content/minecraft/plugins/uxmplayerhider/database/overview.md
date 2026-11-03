---
title: Overview
order: 801
description: What is stored, the table, sharing one database across servers, keeping one server apart, and moving off SQLite.
icon: database
---

The database holds one thing: the state each player chose, for every player whose state is not `all`. A player who
goes back to `all` loses their row, so the table is as long as the number of players who hide anything. The cooldown
is not stored.

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. The default, a file with nothing to set up |
| [MySQL and MariaDB](mysql.md) | A network sharing each player's choice |
| [PostgreSQL](postgresql.md) | The same, on PostgreSQL |

The backend is set in the `storage` block of [config.conf](../config/config-conf.md#storage). It is opened once, so
a change needs a restart. The table is created and moved forward on its own at the start.

## The table

Every table name starts with `storage.table-prefix`, `uxmplayerhider_` by default.

| Table | Holds |
|---|---|
| `uxmplayerhider_state` | One row per player who hides anyone: the player's id and `important` or `none` |
| `uxmlib_schema_history` | The version the table is at |

A player's row is read while they log in, before they arrive, and written off the main thread when they change. A
row that cannot be read is logged, and that player starts showing everybody. `/uxmplayerhider doctor` names the
backend, where the rows are, the prefix and the schema version.

## A network

Point every server at the same MySQL, MariaDB or PostgreSQL database with the same `table-prefix`, and a player's
choice follows them from server to server.

- A change on one server is announced to the others. Each server that has the player online reads the row again
  and applies it at once.
- `sync-seconds` (2 by default) is how often a server reads what the others changed for the players who are on it.
  `0` turns it off.
- `worlds` is each server's own: a lobby can list its worlds while a game server lists none of its own, and the
  choice waits for the player on the lobby.

## Keeping one server apart

`local-only = true` on one server keeps its rows in its own SQLite file, whatever `type` says, under the same prefix.
Use it for a server of the network that should not share the choice, such as a build or test server. Keep the rest of
the `storage` block the same everywhere and set this on that one server alone.

## Moving off SQLite

1. Create the database and a user on the database server.
2. Set `type`, `host`, `port`, `database`, `username` and `password`, and `copy-from-file = true`.
3. Restart once. The rows of the SQLite file are copied into the empty table, and the console says how many.
4. Leave `copy-from-file` on or turn it off: a table that already holds rows is never copied into again, so nothing
   is copied twice.

The SQLite file is left where it is, as a backup.

## Erasing a player

There is no command. A player resets their own choice with `/hide state all`. To erase one by hand, delete their row:
`DELETE FROM uxmplayerhider_state WHERE player_id = '<uuid>';`, with the player offline.

Related: [config.conf](../config/config-conf.md#storage), [SQLite](sqlite.md), [MySQL and MariaDB](mysql.md)
