---
title: Overview
order: 1001
description: What is stored, the tables, sharing one database across servers, and moving off SQLite.
icon: database
---

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. The default |
| [MySQL and MariaDB](mysql.md) | A network sharing prices, stock and player shops |
| [PostgreSQL](postgresql.md) | The same, on PostgreSQL |

## Tables

Every table starts with `table-prefix` (`uxmshop_`).

| Table | Holds |
|---|---|
| `uxmshop_price` | Where each banded line's price stands, and when it last walked home |
| `uxmshop_counter` | Server stock and player allowances, per line, direction and window |
| `uxmshop_modifier` | Player modifiers over everything, a section or a line |
| `uxmshop_shop` | Player and server shops: owner, place, the three switches |
| `uxmshop_offer` | Their offers: the item, prices, currency, match mode and stock |
| `uxmshop_staff` | Who works in which shop |
| `uxmshop_trade` | The log, kept `log.keep-days` |
| `uxmshop_sequence` | Ids handed out |
| `uxmshop_schema_history` | The version the tables are at |

## A network

Point every server at the same database with the same `table-prefix`, and prices, counters, player shops and the log
are shared. A rotation is worked out from the clock, so every server shows the same lines.

## Keeping one server apart

`local-only = true` keeps one server on its own SQLite file, whatever `type` says.

## Moving off SQLite

1. Create the database and a user.
2. Set `type`, `host`, `port`, `database`, `username`, `password`, and `copy-from-file = true`.
3. Restart once. The rows are copied into the empty tables, and the console says how many. Tables that already hold
   rows are left alone.

Related: [config.conf](../config/config-conf.md#storage)
