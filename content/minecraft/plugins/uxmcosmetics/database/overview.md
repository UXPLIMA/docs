---
title: Overview
order: 1001
description: What is stored, the tables, sharing one database across servers, keeping one server apart, and moving off SQLite.
icon: database
---

The database holds what each player owns and what they wear. The pieces, boxes and stations are files.

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. The default |
| [MySQL and MariaDB](mysql.md) | A network sharing wardrobes |
| [PostgreSQL](postgresql.md) | The same, on PostgreSQL |

## Tables

Every table name starts with `storage.table-prefix`, `uxmcosmetics_` by default.

| Table | Holds |
|---|---|
| `uxmcosmetics_owned` | One row per piece a player owns |
| `uxmcosmetics_equipped` | One row per family a player wears a piece in |
| `uxmcosmetics_schema_history` | The version the tables are at |

A piece held by node writes no row.

## A network

Point every server at the same database with the same `table-prefix`. `sync-seconds` (2) is how often a server reads
what the others changed for the players on it. Keep the family files and `boxes.conf` the same everywhere.

## Keeping one server apart

`local-only = true` on one server keeps its rows in its own SQLite file, whatever `type` says.

## Moving off SQLite

1. Create the database and a user.
2. Set `type`, `host`, `port`, `database`, `username`, `password`, and `copy-from-file = true`.
3. Restart once. The rows of the SQLite file are copied into the empty tables, and the console says how many.

Related: [config.conf](../config/config-conf.md#storage), [Networks and cleanup](../features/networks-and-cleanup.md)
