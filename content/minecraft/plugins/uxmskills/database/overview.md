---
title: Overview
order: 1001
description: What is stored, the tables, a network, and moving off SQLite.
icon: database
---

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. The default |
| [MySQL and MariaDB](mysql.md) | A network sharing skills |
| [PostgreSQL](postgresql.md) | The same, on PostgreSQL |

## Tables

Every table starts with `table-prefix` (`uxmskills_`).

| Table | Holds |
|---|---|
| `uxmskills_progress` | Experience and level per player per skill |
| `uxmskills_points` | Ability levels bought with points |
| `uxmskills_choice` | The skills a player chose under specialisation |
| `uxmskills_boost` | Timed boosts, for a player or the server |
| `uxmskills_modifier` | Timed stat modifiers |
| `uxmskills_resource` | Mana and stamina |
| `uxmskills_ledger` | What is paid once and never again: a first discovery and a capstone |
| `uxmskills_board` | Scores per board, period and window |
| `uxmskills_name` | The names the boards show |
| `uxmskills_schema_history` | The version the tables are at |

Placed block marks live in the chunks themselves, not in a table.

## A network

Point every server at the same database with the same `table-prefix`. `sync-seconds` (2) is how often a server reads
what the others changed for the players on it.

## Keeping one server apart

`local-only = true` keeps one server on its own SQLite file, whatever `type` says.

## Moving off SQLite

Set the new `type` and `copy-from-file = true` and restart once: the rows are copied into the empty tables. A backup
written by `/uxmskills backup` is the same file on every backend, so `/uxmskills restore` moves data too.

Related: [config.conf](../config/config-conf.md)
