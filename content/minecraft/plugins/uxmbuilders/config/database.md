---
title: The database
order: 305
description: SQLite or MySQL, what is stored, and moving from one to the other.
icon: database
---

Profiles, builder levels, jobs, warehouses, trust and the audit trail are stored in a database. SQLite
works with no setup; MySQL is for networks or large servers.

## Settings

| Key | Default | Meaning |
|---|---|---|
| `database.type` | `AUTO` | `SQLITE`, `MYSQL`, or `AUTO` to decide from the URL |
| `database.url` | `jdbc:sqlite:./plugins/uxmBuilders/data/uxmbuilders.db` | The JDBC URL |
| `database.username`, `password` | `root`, empty | MySQL only |
| `database.poolSize` | `8` | MySQL connections |
| `database.saveIntervalSeconds` | `300` | How often running builds and warehouses are written |
| `database.crashRecovery` | `true` | Ask owners about unfinished builds after a restart |

For MySQL:

```yaml
database:
  type: 'AUTO'
  url: 'jdbc:mysql://localhost:3306/uxmbuilders'
  username: 'uxmbuilders'
  password: 'change_me_please'
```

MariaDB servers work through a `jdbc:mysql://` URL. A `jdbc:mariadb://` URL is refused with a message
saying so, because the server ships the MySQL driver only.

## What is stored

| Table | Holds |
|---|---|
| `ub_players` | Selected builder, total builds and blocks |
| `ub_player_builders` | Owned builders and their levels |
| `ub_jobs` | Every build: position, rotation, order, progress, price, refund, reason, boost |
| `ub_warehouse` | Material balances |
| `ub_trusted` | Who trusts whom |
| `ub_audit` | Purchases, starts, cancels, refunds, boosts and trust changes, when `logging.audit` is on |
| `ub_schema_version` | The schema version; upgrades are applied in order at startup |

Writes from the build loop go through one background writer in order, so a slow disk never stalls a
tick. State changes (complete, cancel) are written before money moves, so a crash between the two never
refunds twice.

## Moving from SQLite to MySQL

1. Stop the server and keep a copy of `data/uxmbuilders.db`.
2. Point `database.url` at MySQL and start the server. The plugin creates the schema there.
3. Run `/builders admin migrate sqlite-to-mysql`.

It copies the old SQLite file into the live MySQL database. Rows already in MySQL, such as profiles of
players who joined since the switch, are kept. It refuses to run if MySQL already holds builds, and it
leaves the SQLite file untouched.

## Notes

- MySQL servers that require primary keys on every table, the default on several managed hosts, are
  supported.
- `data/` also holds `license.cache`, `instance.id` and `pending-returns.yml`. Delete none of them while
  the server runs.

Related: [Restarts and refunds](../features/recovery.md), [config.yml](config-yml.md)
