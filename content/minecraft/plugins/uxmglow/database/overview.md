---
title: Overview
order: 1001
description: What is stored, the tables, sharing one database across servers, and moving off SQLite.
icon: database
---

The database holds each player's choice: off, a colour, or a cycle with its colours and interval.

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. The default |
| [MySQL and MariaDB](mysql.md) | A network sharing choices |
| [PostgreSQL](postgresql.md) | The same, on PostgreSQL |

## Tables

| Table | Holds |
|---|---|
| `uxmglow_profile` | One row per player: the mode, the colours and the interval |
| `uxmglow_schema_history` | The version the tables are at |

## A network

Point every server at the same database with the same `table-prefix`. `sync-seconds` (2) is how often a server reads
what the others changed for the players on it.

## Keeping one server apart

`local-only = true` keeps one server on its own SQLite file, whatever `type` says.

## Moving off SQLite

1. Create the database and a user.
2. Set `type`, `host`, `port`, `database`, `username`, `password`, and `copy-from-file = true`.
3. Restart once. The rows are copied into the empty tables, and the console says how many.

Related: [config.conf](../config/config-conf.md#storage)
