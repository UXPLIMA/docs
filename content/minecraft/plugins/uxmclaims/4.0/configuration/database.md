---
title: Database
order: 146
description: SQLite, MySQL, MariaDB and PostgreSQL settings, table names, the server lease, backups and /uxm db.
icon: database
---

uxmClaims keeps every land, member and module record in one database, set in the `storage` section of the
root `config.yml`. SQLite needs no setup; a server database is needed when several servers share data. Every
storage setting needs a restart.

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/uxm db status` | Schema version and state of every module | `uxmclaims.admin.db` |
| `/uxm db repair <module> [confirm]` | Clears failed migration rows after you fixed the tables | `uxmclaims.admin.db` |
| `/uxm db baseline <module> <version> [confirm]` | Marks steps as applied for tables restored without history | `uxmclaims.admin.db` |
| `/uxm db remap-world <old> <new> [confirm]` | Moves a world's lands to another world id | `uxmclaims.admin.db` |

Each is described in [Migration commands](../upgrading/migration-commands.md).

## Choosing a type

| `storage.type` | Use it for | Driver settings |
|---|---|---|
| `SQLITE` (default) | One server | `storage.sqlite.file` (`data/storage.db`) |
| `MYSQL` | One server or a network | `storage.remote.*` |
| `MARIADB` | One server or a network | `storage.remote.*` |
| `POSTGRESQL` | One server or a network | `storage.remote.*`, schema `public` |

```yaml
storage:
  type: MYSQL
  remote:
    host: db.example.net
    port: 3306
    database: uxmclaims
    username: uxmclaims
    password: 'secret'
    properties:
      useSSL: 'false'
    jdbc-url-override: ''
  table-prefix: uxmc_
  pool:
    maximum-size: 10
    minimum-idle: 2
    connection-timeout: 30s
    max-lifetime: 30m
```

The port defaults to 3306; write `5432` for PostgreSQL. `properties` are added to the JDBC URL.
`jdbc-url-override` replaces host, port and database with a URL you write in full. Create the database and a
user with rights to create tables before the first start; uxmClaims creates its tables itself. Every key is
listed in [Kernel config](kernel.md).

## Tables

| Table | Holds |
|---|---|
| `<prefix>meta`, `<prefix>players`, `<prefix>player_prefs`, `<prefix>worlds` | The kernel's own data: dataset markers, player names and locales, player preferences, worlds |
| `<prefix>schema_history` | Which schema steps of which module ran, with a checksum per step |
| `<prefix>server_lease` | Which server writes to this data (server databases) |
| `<prefix><module>_<name>` | A module's tables, `-` in the module id written as `_` (`uxmc_lands_chunks`, `uxmc_claim_block_blocks`) |

The prefix is `uxmc_` by default. Do not change `storage.table-prefix` after the first start: the plugin would
create a new, empty set of tables and no longer see the old ones.

Each module brings its own schema steps. At start-up, before modules load data, the kernel runs the pending
steps of every running module in order, one transaction per step, under a lock. A failed step stops only that
module (the kernel's own failure stops the plugin); `/uxm db status` shows it and `/uxm db repair` clears it
once the table is fixed. An edited step file, or a database newer than the jar, stops the module too: install
the newer jar rather than forcing it.

## One server per database

On a server database, the server that starts takes a lease in `<prefix>server_lease` and renews it. A second
server with the same database and table prefix is refused at start-up while the lease is alive (60 seconds
after the holder's last heartbeat). A server whose folder was copied from another, so both carry the same id
(`ip:port`), is refused too. To run several servers on one dataset, turn on [network mode](../network-mode.md):
the servers then share the lease. Servers with their own table prefix are independent datasets.

## Backups

uxmClaims makes database backups at these points:

| When | Where | Format |
|---|---|---|
| The 3.x migration | `backups/pre-4.0-<time>/db/` | A copy of the 3.x SQLite file, or a gzip dump of the `uxmclaims_*` tables |
| An import from another plugin ([Importers](../modules/importers.md)) | `modules/importers/backups/pre-import-<source>-<time>.db` (SQLite) or `.sql.gz` | See below |
| `/uxm migrate restore-v3` | `backups/pre-restore-v3-<time>/` | The 4.x land tables as tab-separated files |

The importer backup is the database backup every module may ask for: on SQLite a full copy of the database file
that the plugin can open again, and on MySQL, MariaDB and PostgreSQL a gzip text dump of every table starting
with the prefix (one `## table` header and a column line per table, tab-separated rows, `\N` for NULL), for a
manual restore. There is no scheduled backup: back up `data/storage.db` or your server database yourself, with
the server stopped or with your database's own dump tool.

## Notes

- The lease timings (60 seconds, renewed every 20) are fixed; an error message mentions
  `storage.lease.heartbeat-seconds`, but that key is not in `config.yml`.
- The PostgreSQL schema is always `public`.
- Moving from SQLite to a server database (or back) is not built in yet.

Related: [Kernel config](kernel.md), [Network mode](../network-mode.md), [Migration commands](../upgrading/migration-commands.md)
