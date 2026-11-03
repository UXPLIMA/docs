---
title: Overview
order: 1001
description: What is stored, sharing one database, and moving off SQLite.
icon: database
---

The database holds the history of every run: its id, the set, when it started and ended, the state, the bytes, the
storages it reached and the reason when something went wrong. The archives themselves are in the storages.

| Table | Holds |
|---|---|
| `uxmbackup_run` | One row per run |
| `uxmbackup_schema_history` | The version the tables are at |

## Keeping one server apart

`local-only = true` keeps one server's history in its own SQLite file, whatever `type` says.

## Moving off SQLite

1. Create the database and a user.
2. Set `type`, `host`, `port`, `database`, `username`, `password`, and `copy-from-file = true`.
3. Restart once. The rows are copied into the empty tables, and the console says how many.

Related: [config.conf](../config/config-conf.md#storage), [SQLite](sqlite.md)
