---
title: Overview
order: 901
description: What is stored, the tables, a network, and moving off SQLite.
icon: database
---

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. The default |
| [MySQL and MariaDB](mysql.md) | A network sharing claims |
| [PostgreSQL](postgresql.md) | The same, on PostgreSQL |

## Tables

Every table starts with `table-prefix` (`uxmsimpleclaims_`).

| Table | Holds |
|---|---|
| `claims` | Every claim: owner, name, home server and world, and the rest of it (members, roles, flags, rules, sub areas, sale, warp) as one document with a version |
| `claim_chunks` | Which chunk belongs to which claim |
| `invites` | Open invites |
| `players` | Per player: name, last seen, inactivity exemption, favourites |
| `payouts` | Money owed to a player who was away |
| `teleports` | Players crossing servers to a claim |
| `inbox` | What happened while a player was away |
| `audit` | Every change to a claim, kept `retention.audit-days` |
| `snapshots` | Deleted claims, kept `retention.snapshots-days` |
| `schema_history` | The version the tables are at |

## A network

Point every server at the same database with the same `table-prefix`, give each a different `network.server-name`,
and either set `network.redis` or let each poll every `sync-seconds` (2).

## Keeping one server apart

`local-only = true` keeps one server on its own SQLite file, whatever `type` says.

## Moving off SQLite

1. Create the database and a user.
2. Set `type`, `host`, `port`, `database`, `username`, `password`, and `copy-from-file = true`.
3. Restart once. The rows are copied into the empty tables, and the console says how many.

Related: [config.conf](../config/config-conf.md#storage)
