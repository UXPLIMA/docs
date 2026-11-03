---
title: Overview
order: 901
description: What is stored, the tables, sharing one database across servers, keeping one server apart, and moving off SQLite.
icon: database
---

The database holds the whole house: every listing, bid, order, held payment, mailbox row, finished sale, watch, the
audit log, each player's totals, the door, and the messages of a network.

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. The default |
| [MySQL and MariaDB](mysql.md) | A network sharing one house |
| [PostgreSQL](postgresql.md) | The same, on PostgreSQL |

## Tables

Every table name starts with `storage.table-prefix`, `auction_` by default.

| Table | Holds |
|---|---|
| `auction_listing` | Every listing: the item, the seller, the price, the kind, the category, the time, the state |
| `auction_bid` | Every bid, and which one leads |
| `auction_buy_order` | Buy orders |
| `auction_escrow` | Money and items the house holds while a bid leads or an order stands |
| `auction_mailbox` | What waits for each player |
| `auction_sale` | One row per finished sale, kept `history.keep-for` |
| `auction_price_point` | The prices of what sold, by item, for history and top prices |
| `auction_player_total` | Each player's sales and earnings, one row per currency |
| `auction_watch` | One row per player per item they wait for |
| `auction_transaction` | The audit log |
| `auction_state` | What the whole house agrees on: the door |
| `auction_message` | The network messages of the `sql` transport, kept `network.keep-for` |

Amounts are stored as text, so no currency loses a digit.

## A network

Point every server at the same database with the same `table-prefix`, and set `network.transport` to `sql` or `redis`;
see [A network](../features/network.md).

## Keeping one server apart

`local-only = true` on one server keeps its rows in its own SQLite file, whatever `type` says, under the same prefix.

## Moving off SQLite

1. Create the database and a user on the database server.
2. Set `type`, `host`, `port`, `database`, `username` and `password`, and `copy-from-file = true`.
3. Restart once. The rows of the SQLite file are copied into the empty tables, and the console says how many.

Tables that already hold rows are left alone, so leaving `copy-from-file` on copies nothing twice. The SQLite file stays
where it is, as a backup.

## Growth

`history.keep-for` trims sales, `network.keep-for` trims messages, and `/uxmauction purge <days>` deletes closed rows.

Related: [config.conf](../config/config-conf.md#storage), [SQLite](sqlite.md)
