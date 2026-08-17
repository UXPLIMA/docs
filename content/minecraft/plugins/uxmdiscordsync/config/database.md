---
title: Database
order: 52
icon: database
---

Five backends. `type` picks one and only that backend's section is read.

```yaml
database:
  type: "flatfile"   # flatfile, sqlite, mysql, mariadb, postgresql
```

| Type | Use when |
|---|---|
| `flatfile` | Single server, small player base, no external database available |
| `sqlite` | Single server, want a real database without running one |
| `mysql` / `mariadb` | Anything shared between servers |
| `postgresql` | Large networks, or where PostgreSQL is what you already run |

Multi-server support requires `mysql`, `mariadb` or `postgresql`. Flatfile and SQLite are files on
one machine and cannot be shared, no matter what `multi-server.enabled` says.

## Flatfile

```yaml
flatfile:
  data-folder: "data"
  pretty-print: true
  backup:
    enabled: true
    interval: 86400
    max-backups: 7
```

JSON files under the plugin folder. `pretty-print: true` makes them readable and larger — worth
keeping while you are setting things up, and worth turning off once you have thousands of links.

Backups are taken every `interval` seconds and `max-backups` are kept. This is the only backend with
built-in backups; the others rely on your own database tooling.

## SQLite

```yaml
sqlite:
  file-name: "database.db"
```

One file in the plugin folder. Do not copy it while the server is running — stop the server first,
or you may get a file that opens and is missing recent writes.

## MySQL and MariaDB

```yaml
mysql:
  host: "localhost"
  port: 3306
  database: "uxmdiscordsync"
  username: "root"
  password: "change_me_please"
  pool:
    maximum-pool-size: 10
    minimum-idle: 5
    connection-timeout: 30000
    max-lifetime: 1800000
    keepalive-time: 60000
  ssl:
    enabled: false
    trust-certificate: true
```

`maximum-pool-size: 10` is generous for this plugin — its queries are small and infrequent. If
several plugins share a database server, the sum of their pools is what matters, and ten connections
each adds up quickly. Five is usually plenty here.

`max-lifetime: 1800000` (30 minutes) must stay **below** your MySQL `wait_timeout`, otherwise the
pool hands out connections the server has already closed. This is the usual cause of a "communications
link failure" an hour after startup.

Set `ssl.enabled: true` for any database not on the same machine. `trust-certificate: true` accepts a
self-signed certificate — acceptable on a private network, not over the internet.

<Callout type="warning" title="Change the default password">

The shipped file has `username: "root"` and `password: "change_me_please"`. Create a dedicated user
with rights to only this database. The plugin never needs `root`, and a shared root password in a
config file is how one compromised plugin folder becomes every database on the host.

</Callout>

## PostgreSQL

```yaml
postgresql:
  host: "localhost"
  port: 5432
  database: "uxmdiscordsync"
  username: "postgres"
  password: "change_me_please"
  schema: "public"
  pool:
    maximum-pool-size: 10
    minimum-idle: 5
    connection-timeout: 30000
    max-lifetime: 1800000
  ssl:
    enabled: false
    mode: "prefer"
```

`schema` lets several plugins share a database cleanly. SSL `mode` takes `disable`, `allow`,
`prefer`, `require`, `verify-ca` or `verify-full`; `require` is the minimum for a remote database and
`verify-full` is the one that actually checks the certificate.

## Checking the connection

```
/uxmdiscordsync database
```

Reports the active type, the connection state and how much data is stored. Run it after any change
here — a database that failed to connect at startup leaves the plugin running with no storage, and
this is the fastest way to see that.

## Switching backends

There is no built-in migration between types. Changing `type` starts an empty store; the old data
stays where it was. Decide before you have players linked, or plan a manual migration.
