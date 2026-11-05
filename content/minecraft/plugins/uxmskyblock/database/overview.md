---
title: Overview
order: 901
description: What is kept, the settings every backend shares, durability and backups.
icon: database
---

The database keeps islands, members and roles, banks and their history, upgrades, warps, homes, missions, seasons,
leaderboards, the game modes' progress and what each player carries. Every money move is written so that a retry
after a lag or a crash is answered rather than applied twice.

## Choosing a backend

`storage.type` in [config.conf](../config/config-conf.md): `sqlite`, `mysql`, `mariadb` or `postgresql`. A change
needs a restart.

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. Nothing to set up |
| [MySQL or MariaDB](mysql.md) | One server, or several sharing islands |
| [PostgreSQL](postgresql.md) | One server, or several sharing islands |

The environment variables `SKYBLOCK_JDBC_URL`, `SKYBLOCK_DB_USER` and `SKYBLOCK_DB_PASSWORD`, when set, are used in
place of the `storage` block, so a password need not be written in a file.

## Settings every server backend reads

| Key | Default | What it does |
|---|---|---|
| `host` | `localhost` | The database server |
| `port` | `0` | The port; 0 is the backend's own, 3306 or 5432 |
| `database` | `minecraft` | The database name |
| `username` | `minecraft` | The user |
| `password` | empty | The password |
| `pool-size` | `10` | Connections kept open |
| `lock-timeout` | `1s` | How long a statement waits for a row another connection holds before it fails and is tried later |

## Durability

At every start the plugin asks the database whether it would keep a commit through a power loss: SQLite below
synchronous FULL, MySQL or MariaDB with `innodb_flush_log_at_trx_commit` other than 1, or PostgreSQL with `fsync` or
`synchronous_commit` off would not. `storage.durability-profile` decides what then happens:

| Value | On a database that would lose a commit |
|---|---|
| `DEVELOPMENT` | Warns in the console and starts |
| `PRODUCTION_STRICT` | Refuses to start |

## Schema

Tables are made and moved forward by numbered migrations the first time a release starts on a database. Nothing is
asked of you. Never edit a table by hand while the server runs.

## Backups

`/is admin backup database` copies the whole database, and `/is restore database <backup>` puts it back after a
typed confirmation. Island backups are separate; see [Lifecycle](../features/lifecycle.md#backups-and-restores).
