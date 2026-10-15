---
title: PostgreSQL
order: 904
description: Creating the database and role, and pointing the plugin at it.
icon: database
---

PostgreSQL 13 or newer.

## Create the database

```sql
CREATE DATABASE uxmclaims;
CREATE USER uxmclaims WITH ENCRYPTED PASSWORD 'a-long-random-password';
GRANT ALL PRIVILEGES ON DATABASE uxmclaims TO uxmclaims;
```

On PostgreSQL 15 and newer the database-level grant is not enough: the user also needs the schema.

```sql
\c uxmclaims
GRANT ALL ON SCHEMA public TO uxmclaims;
```

Without it, the plugin fails at startup on its first table creation with a permission error.

## Point the plugin at it

```yaml
database:
  type: 'AUTO'
  username: 'uxmclaims'
  password: '<your password>'
  url: 'jdbc:postgresql://localhost:5432/uxmclaims'
```

`AUTO` finds `postgres` in the URL. Restart the server: `/claim reload` does not reconnect.

## Remote access

For a database on another host, open it to the game server in `postgresql.conf` and `pg_hba.conf`:

```
listen_addresses = '10.0.0.5'
```

```
host    uxmclaims    uxmclaims    10.0.0.0/24    scram-sha-256
```

Prefer a private network range over `0.0.0.0/0`, and keep `listen_addresses` as narrow as the
setup allows.

## One server only

This database belongs to one server at a time. A second server pointed at it refuses to start while
the first is running; see [One server per database](overview.md). Give every server its own database.

## Backups

```bash
pg_dump -U uxmclaims uxmclaims > uxmclaims-$(date +%F).sql
```

Restore with the server stopped:

```bash
psql -U uxmclaims uxmclaims < uxmclaims-2026-01-01.sql
```

Run a dump before any `/claim admin bulk …` command: they change every loaded claim, without
confirmation and without undo.

Related: [Overview](overview.md), [MySQL](mysql.md)
