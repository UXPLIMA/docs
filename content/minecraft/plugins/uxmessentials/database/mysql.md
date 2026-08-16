---
title: MySQL / MariaDB
order: 1570
description: Pointing uxmEssentials at MySQL or MariaDB, and what changes when you do.
---

When one server outgrows SQLite, or several servers need the same data, point the storage backend at MySQL
or MariaDB. Both speak the MySQL wire protocol, so `backend = "mysql"` drives either.

## Why MySQL / MariaDB?

| Pros                               | Cons                              |
|------------------------------------|-----------------------------------|
| Handles heavy concurrent writes  | You must run a database server  |
| Remote access: many servers, one DB | More to configure          |
| Real row-level locking           | Needs its own backup strategy   |
| Required for cross-server sync   |                                   |

**Recommended for:** busy servers, heavy economies, and any [cross-server network](../cross-server/overview.md).

## Requirements

- MySQL **8.0** or newer, **or** MariaDB **10.5** or newer.
- A database and a user the plugin can connect with.

## Step 1: Create the Database

Connect to your database server and run:

```sql
CREATE DATABASE uxmessentials CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'uxmessentials'@'%' IDENTIFIED BY 'your_secure_password';
GRANT ALL PRIVILEGES ON uxmessentials.* TO 'uxmessentials'@'%';
FLUSH PRIVILEGES;
```

Replace `your_secure_password` with a strong password. Use `'localhost'` instead of `'%'` if
the database runs on the same machine as the server and you do not need remote access.

## Step 2: Configure the Plugin

Edit the `storage` block in `plugins/uxmEssentials/config.conf`:

```hocon
storage {
  backend = "mysql"                 # sqlite | mysql | postgres  (MariaDB uses "mysql")
  host = "localhost"
  port = 3306                       # default 3306 for MySQL / MariaDB
  database = "uxmessentials"
  username = "uxmessentials"
  password = "your_secure_password"
  read-pool-size = 8                # size of the read-connection pool
  connection-timeout-ms = 5000      # how long to wait for a connection before failing
}
```

uxmEssentials builds the JDBC connection for you from `host`, `port` and `database`: there is
no raw URL to paste. For a remote database, point `host` at that machine and make sure its port
(default `3306`) is reachable through your firewall and allowed for the user you created.

| Key | Meaning | Default |
|-----|---------|---------|
| `backend` | Selects the database engine; `mysql` covers MySQL and MariaDB | `sqlite` |
| `host` / `port` | Where the database server listens | `localhost` / `3306` |
| `database` | The schema/database name from Step 1 | `uxmessentials` |
| `username` / `password` | The account from Step 1 | - |
| `read-pool-size` | Concurrent read connections | `8` |
| `connection-timeout-ms` | Wait before a connection attempt fails | `5000` |

## Connection Pool Sizing

Connections are pooled by **HikariCP**. On a network backend there is no single-writer pin:
the database's own row-level locking keeps things safe, so reads and writes run in parallel.

- `read-pool-size = 8` is a sensible default for most servers. Raising it helps a read-heavy
  server (frequent balance/home/warp lookups); there is little point pushing it far past the
  number of cores your database can actually serve.
- Keep `connection-timeout-ms` at `5000` unless you see timeout warnings on a slow or remote
  link: a higher value hides a saturated pool rather than fixing it.

## Migrations Are Automatic

You never create tables by hand. On enable, **Flyway** applies any pending schema migrations to
the database and records what it has run, so upgrades are safe and repeatable. A disabled module
runs none of its migrations. Once the server is up, confirm the connection with:

```
/uxmess doctor
```

It reports the active backend and whether the database is healthy.

## Migrating from SQLite

Switching `backend` from `sqlite` to `mysql` starts a **fresh, empty schema** on the new
database; it does not copy your SQLite data across. There is no built-in file-to-server
converter. To carry data over, use a database export/import tool against each engine, or accept
a clean start on the new backend.

<Callout type="warning" title="Back up first">

Before you change the `backend` value, **back up** your existing `data/uxmessentials.db`
(see [SQLite to Backing Up](sqlite.md#backing-up)). Once the plugin connects to a new,
empty database it will treat that as the source of truth. A backup is your only way back.

</Callout>

## Troubleshooting

| Symptom | Likely cause |
|---------|--------------|
| `Access denied` | Wrong `username`/`password`, or the user lacks privileges on the database |
| `Unknown database` | The database from Step 1 does not exist or is misspelled |
| `Connection refused` | The database is not running, or `host`/`port`/firewall is wrong |
| Connection times out on join | `read-pool-size` too small for load, or a slow remote link |

Related: [PostgreSQL Setup](postgresql.md), [Cross-Server](../cross-server/overview.md), [config.conf (Globals)](../config/global-config.md), [Economy](../modules/economy.md)
