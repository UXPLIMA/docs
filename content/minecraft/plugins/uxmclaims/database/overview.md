---
title: Overview
order: 901
description: Choosing a backend, the database settings, the tables, and the one-server rule.
icon: database
---

uxmClaims stores claims and player records in SQLite, MySQL or PostgreSQL. It creates and updates
its own tables at startup, then loads every claim into memory and serves reads from there.

## Settings

```yaml
database:
  type: 'AUTO'
  username: 'root'
  password: 'password'
  url: 'jdbc:sqlite:./plugins/uxmClaims/data/claims.db'
```

| Key | Default | Meaning |
|---|---|---|
| `type` | `AUTO` | `AUTO`, `SQLITE`, `MYSQL` or `POSTGRES` |
| `username` | `root` | Database user, used by MySQL and PostgreSQL only |
| `password` | `password` | Its password, used by MySQL and PostgreSQL only |
| `url` | `jdbc:sqlite:./plugins/uxmClaims/data/claims.db` | The JDBC URL |

`AUTO` looks for `mysql`, `postgres` or `sqlite` in the URL, in that order, and refuses to start if
it finds none. The drivers and the HikariCP pool are bundled. The pool holds up to 64 connections and
has no settings of its own.

The whole section is read once, at startup. `/claim reload` re-reads `config.yml` but keeps the
connection it has: changing the backend or the credentials needs a restart.

| Backend | Right for |
|---|---|
| [SQLite](sqlite.md) | Most servers. The default, a single local file, no setup |
| [MySQL](mysql.md) | A database you already run and back up centrally, or query from a web panel |
| [PostgreSQL](postgresql.md) | The same, where PostgreSQL is the house standard |

MySQL 8.0 or newer and PostgreSQL 13 or newer are supported.

## One server per database

Claims are cached in memory and the write locks are local to one server, so two servers writing to
the same database would silently overwrite each other. On MySQL and PostgreSQL the plugin therefore
holds a lease: a single row in `uxmclaims_server_lease` naming the server that owns the data.

| Situation | Result |
|---|---|
| The lease is free, or held by this same server | The server takes it and starts |
| Another live server holds it | The plugin logs `Another server (…) is already using this database.` and disables itself |
| The holder stopped cleanly | The lease is released at shutdown; the next server starts at once |
| The holder crashed | Its lease is taken over once its heartbeat is 60 seconds old |
| The lease is taken over while this server runs | The plugin logs it and disables itself |

The holder renews the lease every 20 seconds. A server is identified by the id in
`plugins/uxmClaims/data/server.id` plus its port, so a restarted server takes its own lease back
immediately. Copying the plugin folder to a second server copies that id too: delete `data/server.id`
on the copy. SQLite needs no lease, because the file is local.

To run claims on several servers, give each server its own database.

## Tables

Every table has the prefix `uxmclaims_`.

| Table | Holds |
|---|---|
| `players` | Player name, skin and login dates |
| `claims` | Name, spawn, creation and expiry dates |
| `claim_chunks` | The chunks of each claim |
| `claim_flags` | The flags set on each claim |
| `claim_members` | Members and their role |
| `claim_member_allowed_permissions` / `claim_member_denied_permissions` | Per-member overrides |
| `claim_roles` / `claim_role_permissions` | Roles and their permissions |
| `claim_invites` / `claim_bans` | Pending invites and bans |
| `claim_warps` | Warps, unique per claim and name |
| `claim_blocks` | The claim block style and position |
| `claim_vaults` / `claim_vault_items` | Vault contents |
| `server_lease` | The lease row (MySQL and PostgreSQL) |

Two views, `uxmclaims_view_claims_json` and `uxmclaims_view_players_json`, return each claim and
player as one JSON document, which is the easiest way to read the data from outside the game.

## Migrations

The schema is versioned by the files under `migrations/<backend>/` in the plugin jar, and every start
applies the versions the database has not seen. Each version runs in one transaction.

| Version | Change |
|---|---|
| v1 | The tables and the two views |
| v2 | Removes two retired flags |
| v3 | Makes warp names unique per claim instead of server-wide |
| v4 | Splits member permissions into allowed and denied tables |
| v5 | Adds `uxmclaims_server_lease` |
| v6 | Grants `USE_WARPS` once to every `MEMBER` and `CUSTOM` role, so members keep private warp access; owners can remove it afterwards |

Applied versions are recorded in `schema_migrations` with a checksum. Do not edit that table or the
schema by hand: a checksum that no longer matches stops the plugin at startup.

## Moving between backends

There is no migration command. Changing the URL points the plugin at an empty database; the claims
do not follow. To move, export from the old database and import into the new one with your
database's own tools, with the server stopped.

<Callout type="warning" title="A new URL is an empty database">

Pointing `url` at a fresh database means every player's claims are gone until you point it back.
Take a copy of `data/claims.db`, or a dump, before touching this section on a live server.

</Callout>

## Backups

| Backend | Backup |
|---|---|
| SQLite | `sqlite3 plugins/uxmClaims/data/claims.db ".backup 'claims-backup.db'"`, safe while running |
| MySQL | `mysqldump -u uxmclaims -p uxmclaims > backup.sql` |
| PostgreSQL | `pg_dump -U uxmclaims uxmclaims > backup.sql` |

Take one before any `/claim admin bulk …` command: they change every loaded claim, without
confirmation and without undo.

Related: [SQLite](sqlite.md), [MySQL](mysql.md), [PostgreSQL](postgresql.md), [config.yml](../config/config-yml.md)
