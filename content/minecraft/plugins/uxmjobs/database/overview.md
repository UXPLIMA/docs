---
title: Overview
order: 1001
description: What is stored, the tables, sharing one database across servers, and moving off SQLite.
icon: database
---

The database holds job levels, the archive, points, the placed block ledger, decay counters, income windows, the timed
boost, signed contracts, zone blocks owed back, explored chunks, payouts, signs and the boards.

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. The default |
| [MySQL and MariaDB](mysql.md) | A network sharing jobs |
| [PostgreSQL](postgresql.md) | The same, on PostgreSQL |

## Tables

| Table | Holds |
|---|---|
| `uxmjobs_employment` | One row per player per job held: the experience |
| `uxmjobs_archive` | One row per job a player left: the experience kept and how often they left |
| `uxmjobs_points` | The points balance |
| `uxmjobs_ledger` | Placed blocks and spots already paid, kept `ledger.retention-days` |
| `uxmjobs_decay` | Repeat counts per player, material and region |
| `uxmjobs_income` | Earnings per player per currency in the income window |
| `uxmjobs_booster` | The timed boost |
| `uxmjobs_contract` | Signed contracts and their points |
| `uxmjobs_zone_block` | Blocks a zone owes back |
| `uxmjobs_explored` | Chunks visited, for `explore` |
| `uxmjobs_payout` | Payments, for the report, kept `report.retention-days` |
| `uxmjobs_sign` | Job signs |
| `uxmjobs_board`, `uxmjobs_name` | Board standings per period, and the names they show |
| `uxmjobs_schema_history` | The version the tables are at |

## A network

Point every server at the same database with the same `table-prefix`. `sync-seconds` (2) is how often a server reads
what the others changed for the players on it: a level earned or a booster started there. The
income caps, decay counters and ledger are shared state too, so a network that splits them splits the protections.

## Keeping one server apart

`local-only = true` keeps one server on its own SQLite file, whatever `type` says.

## Moving off SQLite

1. Create the database and a user.
2. Set `type`, `host`, `port`, `database`, `username`, `password`, and `copy-from-file = true`.
3. Restart once. The rows are copied into the empty tables, and the console says how many.

Related: [config.conf](../config/config-conf.md#storage)
