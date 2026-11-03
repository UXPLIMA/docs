---
title: "Storage"
order: 50
description: "Boards, totals, contributions and pass standings live in a database."
---

Boards, totals, contributions and pass standings live in a database. The `storage` block of `config.conf`
chooses it.

| Key | Meaning |
|---|---|
| `type` | `sqlite`, `mysql`, `mariadb` or `postgresql`. |
| `file` | The SQLite file inside the plugin folder. Read only for `sqlite`. |
| `host`, `port`, `database`, `username`, `password` | The database server. Port `0` is the server's own: 3306 for MySQL and MariaDB, 5432 for PostgreSQL. |
| `table-prefix` | Written before every table name, so two plugins or two copies can share a database. Letters, digits and underscores, at most 32. |
| `pool-size` | Connections the pool may open. SQLite uses one. |
| `sync-seconds` | How often this server reads what other servers on the same database wrote for a player who is here. `0` turns it off. SQLite reads nothing. |
| `join-delay-ticks` | How long to wait after a join before reading the player. |
| `local-only` | This server keeps its rows in the SQLite file whatever `type` says. |
| `copy-from-file` | Copies the SQLite rows into empty network tables once. |

SQLite is the default, needs no setup, and is right for one server. The schema is created and upgraded on
start; nothing is done by hand when a release changes it.

## A network of servers

Point every server at one database and keep the `storage` block the same on all of them. Each server reads
what the others wrote every `sync-seconds`.

A proxy connects a player to the next server before the previous one has written them. `join-delay-ticks` (20
is one second) makes that race rarer but does not close it; the answer that closes it is one shared database.
A player counts nothing during the wait.

A server that has no business in the shared database, such as a creative world, sets `local-only = true`.

## Moving from SQLite to a network database

Set `type` to the new database, set `copy-from-file = true` and restart once. The rows of the file are copied
into the empty tables and the log says how many. Tables that already hold rows are left alone, so leaving the
switch on copies nothing twice.

`/uxmquests doctor` prints the backend, the place, the prefix and the schema version in use.
