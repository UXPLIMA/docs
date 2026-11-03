---
title: Overview
order: 1001
description: What is stored, the tables, sharing one database across servers, and moving off SQLite.
icon: database
---

The database holds plots and everything said about them. Snapshots, templates and archived builds are files under
`snapshots/`, `templates/` and `archive/`, not rows.

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. The default |
| [MySQL and MariaDB](mysql.md) | A network sharing plots |
| [PostgreSQL](postgresql.md) | The same, on PostgreSQL |

## Tables

Every table starts with `table-prefix` (`uxmplots_`).

| Table | Holds |
|---|---|
| `plot`, `member`, `ban`, `merge`, `flag` | Plots, their owners, names, members, bans, merges and flags |
| `slot` | Room bought with `/plot slots buy` |
| `visit`, `rating`, `rating_category` | Visits and ratings |
| `tag`, `featured`, `personality`, `unlock` | Tags, pinned and sponsored rows, looks, bought catalogue entries |
| `note`, `tip`, `follow`, `report` | Guestbook notes, tips, following, reports |
| `listing`, `sale`, `rent`, `tenancy` | The market |
| `entry`, `entry_builder`, `vote`, `result`, `result_builder` | Contest entries, teams, votes and results |
| `contest_result`, `contest_result_builder` | Places for honours and levels |
| `archive`, `archive_builder` | The hall of fame |
| `change` | What changed, for the other servers on the `sql` transport, swept by age (`keep-minutes`) |
| `owed_reward` | Rewards owed to a player who was away |
| `server_world`, `travel`, `last_seen` | The network: which server has which world, players crossing, when each was last seen |
| `schema_history` | The version the tables are at |

## A network

Point every server at the same database with the same `table-prefix`, and set `network.conf` so the servers hear what
changed; see [Network and Discord](../features/network-and-discord.md#the-network).

## Keeping one server apart

`local-only = true` keeps one server on its own SQLite file, whatever `type` says: a build server that has no business
in the shared database.

## Moving off SQLite

1. Create the database and a user.
2. Set `type`, `host`, `port`, `database`, `username`, `password`, and `copy-from-file = true`.
3. Restart once. The rows are copied into the empty tables, and the console says how many. Tables that already hold
   rows are left alone.

Related: [config.conf](../config/config-conf.md#storage)
