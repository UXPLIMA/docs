---
title: Database
order: 54
description: SQLite or MySQL, backups, and what happens to the database on an upgrade.
icon: database
---

## Choosing a database

| | SQLite | MySQL or MariaDB |
|---|---|---|
| Setup | None: a file in the plugin folder | A database and a user |
| One server | Right choice | Works |
| A network | Not possible | Required |

SQLite is the default and keeps everything in `supplyrush_database.db` (named after `database` in
`config.yml`). For MySQL or MariaDB, create the database with full character support first, so
event and player names keep every character:

```sql
CREATE DATABASE supplyrush CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

Then set `driver: "MYSQL"` and the connection details in [config.yml](config-yml.md), and restart.
The tables are created on the first start. `use-extra-flags` should stay `false` unless UXPLIMA
support tells you otherwise.

## What the database holds

Events and their items, the state of the running event, every deposit, the history of finished
runs, rewards waiting for offline winners, and the in-game logs. Your configuration files stay in
the plugin folder.

There is no built-in way to move existing data from SQLite to MySQL. Pick the database before your
first tournament, or open a ticket with UXPLIMA support if you need to move.

## Backups

Back up the plugin folder and, on MySQL, the database, at the same time.

<Callout type="warning" title="Do not copy the SQLite file from a running server">

The file is written to while the server runs, and a copy taken then can be missing the latest
deposits. Stop the server first, then copy `plugins/uxmSupplyRush/`. On MySQL, your usual dump
tools work while the server runs.

</Callout>

## Upgrades

When a new version needs a change to the database, it makes the change by itself on start and says
so in the console. With SQLite, the old file is first copied to `backups/`. With MySQL, take your
own backup before upgrading. See [Upgrading to 1.1.0](../getting-started/upgrading.md).

Related: [config.yml](config-yml.md), [Multiple servers](../features/multi-server.md)
