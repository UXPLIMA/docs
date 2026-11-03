---
title: Overview
order: 1001
description: What is stored, the one table, sharing one database across servers, keeping one server apart, and moving off SQLite.
icon: database
---

The database holds one row per player the server has seen: the language they chose, and the one their client last
reported. The client's language is kept too, so the next server a player reaches opens in the right language before
their first line is drawn, even when they never chose one.

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. The default |
| [MySQL and MariaDB](mysql.md) | A network sharing language choices |
| [PostgreSQL](postgresql.md) | The same, on PostgreSQL |

## Tables

Every table name starts with `storage.table-prefix`, `uxmlang_` by default.

| Table | Holds |
|---|---|
| `uxmlang_player` | `player_id`, `chosen` (the language the player chose, or empty) and `client` (the language their client last reported) |
| `uxmlang_schema_history` | The version the tables are at |

## A network

Point every server at the same database with the same `table-prefix`, and a player's language follows them. With
Redis, a change also reaches the server they are on at once; see [A network](../features/network.md).

## Keeping one server apart

`local-only = true` on one server keeps its rows in its own SQLite file, whatever `type` says, under the same prefix.
Keep the storage block otherwise the same on every server.

## Moving off SQLite

1. Create the database and a user on the database server.
2. Set `type`, `host`, `port`, `database`, `username` and `password`, and `copy-from-file = true`.
3. Restart once. The rows of the SQLite file are copied into the empty tables, and the console says how many.

Tables that already hold rows are left alone, so leaving `copy-from-file` on copies nothing twice. The SQLite file
stays where it is, as a backup.

Related: [config.conf](../config/config-conf.md#storage), [SQLite](sqlite.md), [MySQL and MariaDB](mysql.md)
