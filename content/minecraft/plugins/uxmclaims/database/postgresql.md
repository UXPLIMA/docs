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

On PostgreSQL 15 and newer, the database-level grant is not enough — the user also needs the schema:

```sql
\c uxmclaims
GRANT ALL ON SCHEMA public TO uxmclaims;
```

Without that, the plugin starts and fails on its first table creation with a permission error. It is
the single most common PostgreSQL setup problem.

## Point the plugin at it

```yaml
database:
  type: 'AUTO'
  username: 'uxmclaims'
  password: 'a-long-random-password'
  url: 'jdbc:postgresql://localhost:5432/uxmclaims'
```

`AUTO` reads `postgresql` out of the URL. Restart — this section is not re-read by `/claim reload`.

## Remote access

`postgresql.conf`:

```
listen_addresses = 'localhost'
```

`pg_hba.conf`, for a database on another host:

```
host    uxmclaims    uxmclaims    10.0.0.0/24    scram-sha-256
```

Prefer a private network range over `0.0.0.0/0`, and keep `listen_addresses` as tight as the topology
allows.

## Backups

```bash
pg_dump -U uxmclaims uxmclaims > uxmclaims-$(date +%F).sql
```

Restore:

```bash
psql -U uxmclaims uxmclaims < uxmclaims-2026-01-01.sql
```

Run a dump before any `/claim admin bulk …` command — they rewrite every loaded claim, without
confirmation and without undo.
