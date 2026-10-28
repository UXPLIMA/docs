---
title: Database
order: 52
description: The five storage types, connecting to MySQL and PostgreSQL, and moving an old flatfile server to SQL.
icon: database
---

The database holds the links, the link codes, levels, boost cooldowns and the history of links,
boost claims and rank syncs. It is set in the `database` section of `config.yml`, and only the section
of the chosen `type` is read.

```yaml
database:
  type: "sqlite"
```

| `type` | Use it when |
|---|---|
| `sqlite` | One server. A single file, nothing to install. The default |
| `mysql`, `mariadb` | Several servers share the bot, or you already run MySQL or MariaDB |
| `postgresql` | Several servers share the bot and PostgreSQL is what you run |
| `flatfile` | Only for servers that already use it. Use `sqlite` instead |

Several servers sharing one bot need `mysql`, `mariadb` or `postgresql`: SQLite and flatfile are files
on one machine, and the plugin does not start on them in multi-server mode.

Changing the database needs a restart. If the database cannot be reached at startup, or `type` is
misspelled, the plugin does not start, and the console says why; it never runs on an empty
replacement where every player would look unlinked.

## SQLite

```yaml
  sqlite:
    file-name: "database.db"
```

The file is in the plugin folder. Copy it only while the server is stopped; a copy taken while it
runs can miss the last changes.

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
      connection-timeout: 10000
      max-lifetime: 1800000
      keepalive-time: 60000
    ssl:
      enabled: false
      trust-certificate: true
```

`mariadb` reads the same `mysql` section. Create the database and a user for it first; the plugin
creates its own tables, all starting with `uxm_`.

```sql
CREATE DATABASE uxmdiscordsync CHARACTER SET utf8mb4;
CREATE USER 'uxmds'@'%' IDENTIFIED BY 'a-strong-password';
GRANT ALL PRIVILEGES ON uxmdiscordsync.* TO 'uxmds'@'%';
```

| Setting | Meaning |
|---|---|
| `maximum-pool-size` | Connections kept open. The plugin's queries are small; 5 is usually enough, and every plugin's pool counts against the database's limit |
| `connection-timeout` | Milliseconds to wait for a connection before giving up |
| `max-lifetime` | Milliseconds before a connection is replaced. Keep it below the database's `wait_timeout`, or the plugin is handed connections the database already closed |
| `ssl.enabled` | Always encrypt the connection; a database without TLS is refused |
| `ssl.trust-certificate` | `true` encrypts without checking the certificate, so a self-signed one works. `false` also checks that the certificate is trusted and matches the host |

Turn on `ssl` for a database on another machine. `trust-certificate: true` is fine on a private
network, not across the internet.

The shipped `root` and `change_me_please` are placeholders. Give the plugin a user with rights to its
own database only, never `root`.

## PostgreSQL

```yaml
  postgresql:
    host: "localhost"
    port: 5432
    database: "uxmdiscordsync"
    username: "postgres"
    password: "change_me_please"
    schema: "public"
    pool: ...
    ssl:
      enabled: false
      mode: "prefer"
```

`schema` lets the plugin keep its tables apart in a database other plugins use. `ssl.mode` takes
`disable`, `allow`, `prefer`, `require`, `verify-ca` or `verify-full`: `require` is the least for a
remote database, and `verify-full` the one that checks the certificate.

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

JSON files in `data/`. A copy is made into `data/backups/` when data is saved and `interval` seconds
have passed since the last one, and the newest `max-backups` are kept. If a file cannot be read, the
plugin does not start and points at the backups.

## Moving off flatfile

The import copies links, link history, levels, level rewards taken, boost cooldowns and the logs from
flatfile into the SQL database you switch to. Link codes are left out; they expire in minutes.

1. Put the server in maintenance, for example with the whitelist. A player who joins between the
   switch and the import can claim rewards again and lose XP.
2. Set `database.type` to `sqlite`, `mysql`, `mariadb` or `postgresql`, fill in its section, and
   restart.
3. Run `/dsadmin database importflatfile`. It counts what it found and changes nothing.
4. Run `/dsadmin database importflatfile confirm` within 30 seconds.

While it runs, the plugin does nothing else: logins are refused, commands answer that an import is
running, and Discord is ignored. When it is done, it says how much it copied. Lift the maintenance.

Everything goes in one step: if the import fails, nothing is copied. Rows the SQL database already
has, such as a link made since the switch, are kept over the flatfile ones. The flatfile files stay
where they are, and `data/imported-to-sql.txt` next to them stops a second import; delete it to import
again.

## Checking

`/dsadmin stats` shows the database type and the number of linked accounts. A count of zero right
after a change of database means the plugin is reading a new, empty one.

Related: [Multi-server](../features/multi-server.md), [Upgrading to 2.2.0](../getting-started/upgrading.md)
