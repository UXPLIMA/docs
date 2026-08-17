---
title: MySQL
order: 903
description: Creating the database and user, and pointing the plugin at it.
icon: database
---

MySQL 8 or newer. MariaDB works through the same dialect.

## Create the database

```sql
CREATE DATABASE uxmclaims CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'uxmclaims'@'localhost' IDENTIFIED BY 'a-long-random-password';
GRANT ALL PRIVILEGES ON uxmclaims.* TO 'uxmclaims'@'localhost';
FLUSH PRIVILEGES;
```

`utf8mb4` matters — claim names contain emoji more often than you would expect, and `utf8` in MySQL
is a three-byte encoding that cannot store them.

Use `'uxmclaims'@'%'` instead of `@'localhost'` when the database is on another host, and restrict it
at the firewall rather than trusting the wildcard.

## Point the plugin at it

```yaml
database:
  type: 'AUTO'
  username: 'uxmclaims'
  password: 'a-long-random-password'
  url: 'jdbc:mysql://localhost:3306/uxmclaims'
```

`AUTO` reads `mysql` out of the URL. Restart the server — this section is not re-read by
`/claim reload`.

Useful URL parameters:

```
jdbc:mysql://localhost:3306/uxmclaims?useSSL=false&characterEncoding=utf8&serverTimezone=UTC
```

Set `serverTimezone` when the database host and the game server disagree about time. Claim expiry is
stored as an instant, but a mismatched session timezone makes every `date:` placeholder read wrong.

## Several servers

More than one server may share one database. That is the main reason to be here.

What is shared: claims, members, roles, warps, bans, vaults. What is not: the in-memory caches. A claim
edited on one server is not instantly visible on another — there is no cross-server invalidation
message. Give each server its own worlds, and the question does not arise.

## Backups

```bash
mysqldump -u uxmclaims -p uxmclaims > uxmclaims-$(date +%F).sql
```

Run it before any bulk operation. `/claim admin bulk rename` has no undo and no confirmation.

<Callout type="warning" title="Change the shipped credentials">

`config.yml` ships with `username: 'root'` and `password: 'password'`. They do nothing under SQLite,
which is the default, so it is easy to switch the URL to MySQL and forget them. Set both.

</Callout>
