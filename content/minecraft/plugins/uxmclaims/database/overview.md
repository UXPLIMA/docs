---
title: Overview
order: 901
description: Choosing a backend, and what AUTO does.
icon: database
---

```yaml
database:
  type: 'AUTO'
  username: 'root'
  password: 'password'
  url: 'jdbc:sqlite:./plugins/uxmClaims/data/claims.db'
```

`type: 'AUTO'` reads the dialect out of the JDBC URL, which is what you want — the URL already says
`sqlite`, `mysql`, `mariadb` or `postgresql`.

| Backend | Right for |
|---|---|
| [SQLite](sqlite.md) | A single server. The default, and fine for most. |
| [MySQL](mysql.md) | Several servers, or a database you already back up centrally. |
| [PostgreSQL](postgresql.md) | The same, where Postgres is the house standard. |

MySQL 8+ and PostgreSQL 13+ are supported. MariaDB works through the MySQL dialect.

## Choosing

SQLite is not a compromise on one server. It is a file, it is fast, and it needs no maintenance. Move
to MySQL or PostgreSQL when one of these is true:

- more than one server needs the same claims
- your backups are database-level, not file-level
- you want to query claim data from outside the game

## Moving between them

There is no migration command. Changing the URL points the plugin at an empty database; the claims do
not follow.

To move for real: export from the old database and import into the new one with the usual tools —
`sqlite3 .dump` into a converter for SQLite, `mysqldump` or `pg_dump` for the others — with the server
stopped on both sides.

<Callout type="warning" title="database is read once, at startup">

`/claim reload` re-reads every other file but leaves the connection alone. Changing the backend needs
a full restart, and pointing at a fresh database means every player's claims are gone until you point
back.

Take a copy of `data/claims.db` before touching this section on a live server.

</Callout>

## Backups

| Backend | Backup |
|---|---|
| SQLite | Copy `plugins/uxmClaims/data/claims.db` with the server stopped |
| MySQL | `mysqldump uxmclaims > backup.sql` |
| PostgreSQL | `pg_dump uxmclaims > backup.sql` |

Copying a SQLite file while the server is running can capture a half-written state. Stop, copy, start
— or use `sqlite3 claims.db ".backup 'out.db'"`, which is safe against a live connection.
