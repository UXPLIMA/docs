---
title: MySQL
order: 903
description: Creating the database and user, and pointing the plugin at it.
icon: database
---

MySQL 8.0 or newer. The schema uses MySQL 8 features (`BIN_TO_UUID` in its views and the
`utf8mb4_0900_ai_ci` collation), so use MySQL itself rather than MariaDB.

## Create the database

```sql
CREATE DATABASE uxmclaims CHARACTER SET utf8mb4;
CREATE USER 'uxmclaims'@'localhost' IDENTIFIED BY 'a-long-random-password';
GRANT ALL PRIVILEGES ON uxmclaims.* TO 'uxmclaims'@'localhost';
```

The plugin creates its tables and views itself, so the user needs `CREATE`, `ALTER`, `DROP` and
`CREATE VIEW` on the database as well as read and write access.

Use `'uxmclaims'@'%'` instead of `'localhost'` when the database runs on another host, and restrict
access at the firewall rather than trusting the wildcard.

## Point the plugin at it

```yaml
database:
  type: 'AUTO'
  username: 'uxmclaims'
  password: '<your password>'
  url: 'jdbc:mysql://localhost:3306/uxmclaims'
```

`AUTO` finds `mysql` in the URL. Keep the `jdbc:mysql://` scheme: the bundled driver is MySQL
Connector/J. Driver options go after a `?`, for example `?useSSL=false` for a local database
without TLS.

Restart the server: `/claim reload` does not reconnect.

## One server only

This database belongs to one server at a time. A second server pointed at it refuses to start while
the first is running; see [One server per database](overview.md). Give every server its own database.

## Backups

```bash
mysqldump -u uxmclaims -p uxmclaims > uxmclaims-$(date +%F).sql
```

Run it before any `/claim admin bulk …` command. They have no undo and no confirmation.

<Callout type="warning" title="Change the shipped credentials">

`config.yml` ships with `username: 'root'` and `password: 'password'`. SQLite ignores them, so it is
easy to switch the URL to MySQL and forget them. Set both.

</Callout>

Related: [Overview](overview.md), [PostgreSQL](postgresql.md)
