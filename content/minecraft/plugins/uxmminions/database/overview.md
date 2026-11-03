---
title: Overview
order: 901
description: What is stored, the tables, sharing one database, keeping one server apart, and moving off SQLite.
icon: database
---

The database holds every minion: its row, its stock, its bank, its tool, who it trusts, what it produced and paid, and
who moved it. Types, fuel, prices and skins are files.

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. The default |
| [MySQL and MariaDB](mysql.md) | A network that keeps minions in one place |
| [PostgreSQL](postgresql.md) | The same, on PostgreSQL |

## Tables

Every table name starts with `storage.table-prefix`, `minions_` by default.

| Table | Holds |
|---|---|
| `minions_minion` | One row per minion: owner, type, level, world and position, chunk, facing, name, skin, fuel, actions done, destination and linked container, scope |
| `minions_stock` | Its virtual contents; never in a block beside it |
| `minions_filter` | Materials it was told to stop collecting |
| `minions_trust` | Who may use a minion they do not own |
| `minions_ledger` | What each minion produced and paid |
| `minions_audit` | Every placement, break, pickup, upgrade, restore and refused duplicate |
| `minions_grant` | Extra slots an operator granted |
| `minions_bank` | Money held for collection |
| `minions_tool`, `minions_given_tool` | Tool wear, and the tool a player gave |
| `minions_stored` | Minions picked up into the pocket, with where they last stood |
| `minions_schema_history` | The version the tables are at |

## A network

Point every server at the same database with the same `table-prefix`, and every minion, its stock and its bank are in
one place. A minion is worked by the server its chunk is loaded on; there is no `sync-seconds`.

## Keeping one server apart

`local-only = true` on one server keeps its rows in its own SQLite file, whatever `type` says, under the same prefix.

## Moving off SQLite

1. Create the database and a user on the database server.
2. Set `type`, `host`, `port`, `database`, `username` and `password`, and `copy-from-file = true`.
3. Restart once. The rows of the SQLite file are copied into the empty tables, and the console says how many.

Tables that already hold rows are left alone, so leaving `copy-from-file` on copies nothing twice.

Related: [config.conf](../config/config-conf.md#storage)
