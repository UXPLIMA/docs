---
title: The database
order: 405
description: SQLite, MySQL, MariaDB or PostgreSQL, what is stored, several servers on one database, and moving from SQLite.
icon: database
---

Profiles, builder levels, jobs, warehouses, trust, the player market and the audit trail are stored in a
database. SQLite
works with no setup; MySQL 8+, MariaDB and PostgreSQL 13+ are for networks or large servers. The drivers
are downloaded on the first start, into `libs/`.

## Settings

| Key | Default | Meaning |
|---|---|---|
| `database.type` | `AUTO` | `SQLITE`, `MYSQL`, `POSTGRES`, or `AUTO` to decide from the URL |
| `database.url` | `jdbc:sqlite:./plugins/uxmBuilders/data/uxmbuilders.db` | The JDBC URL |
| `database.username`, `password` | `root`, empty | Not used by SQLite |
| `database.poolSize` | `8` | Connections in the pool, at least 2; not used by SQLite |
| `database.saveIntervalSeconds` | `300` | How often running builds are checkpointed |
| `database.crashRecovery` | `true` | Ask owners about unfinished builds after a restart |
| `database.serverId` | `default` | This server's name in a database shared with others |

<Tabs>
<Tab label="MySQL or MariaDB">

```yaml
database:
  type: 'AUTO'
  url: 'jdbc:mysql://localhost:3306/uxmbuilders'
  username: 'uxmbuilders'
  password: 'change_me_please'
```

A `jdbc:mariadb://` URL works too: it is handed to the MySQL driver as `jdbc:mysql://`.

</Tab>
<Tab label="PostgreSQL">

```yaml
database:
  type: 'AUTO'
  url: 'jdbc:postgresql://localhost:5432/uxmbuilders'
  username: 'uxmbuilders'
  password: 'change_me_please'
```

</Tab>
</Tabs>

With SQLite, a URL that is empty or not a `jdbc:sqlite:` URL falls back to `data/uxmbuilders.db`. The
file's folder is created if it is missing.

## What is stored

| Table | Holds |
|---|---|
| `uxmbuilders_players` | Selected builder, total builds and blocks |
| `uxmbuilders_player_builders` | Owned builders and their levels |
| `uxmbuilders_jobs` | Every build: server, position, rotation, order, progress, price, refund, reason, pause reason, boost, and the design it builds |
| `uxmbuilders_warehouse` | Material balances |
| `uxmbuilders_trust` | Who trusts whom |
| `uxmbuilders_audit` | Purchases, starts, cancels, refunds, boosts, trust, admin changes and market activity, when `logging.audit` is on |
| `uxmbuilders_market_snapshots` | The designs listed on the market, each stored once |
| `uxmbuilders_market_listings` | Listings: seller, title, price, stock, status, the commission rate fixed when listed, where the design was captured, reports |
| `uxmbuilders_market_purchases` | Every purchase: buyer, price, commission, builds used, the blueprint's current copy |
| `uxmbuilders_market_earnings` | What the market owes and paid each seller and the commission account |
| `uxmbuilders_market_reports` | Reports and how they were resolved |
| `uxmbuilders_market_notifications` | Sales and decisions waiting for players who were away |
| `uxmbuilders_market_bans` | Market bans |
| `uxmbuilders_schema_migrations` | The schema version; upgrades are applied in order at startup |

The `uxmbuilders_` prefix keeps the tables apart from other plugins', so uxmBuilders can share a database
with uxmClaims. Tables a 1.0.0 server left behind (`ub_*`) are not read; see
[Upgrading from 1.0.0](../getting-started/upgrading.md).

Checkpoints go through one background writer in order, so a slow disk never stalls a tick. The end of a
build is written before money moves or the build is counted: a refund is paid only once the cancelled
build is stored, so a crash between the two never refunds twice. If the database refuses that write,
nothing is paid, the owner is told the refund failed, and the build comes back after the next restart.

## Schema upgrades

The schema version is kept in `uxmbuilders_schema_migrations`. At startup the plugin applies, in order,
every upgrade the database has not had yet, the same way on SQLite, MySQL and PostgreSQL.

| Version | Adds |
|---|---|
| 1 | The tables for players, builders, builds, warehouses, trust and the audit trail |
| 2 | The design each build builds: a package, or a design bought on the market |
| 3 | The player market's tables |
| 4 | `commission_bp` in `uxmbuilders_market_listings`: the commission each listing keeps, in hundredths of a percent, fixed when it is listed. Listings stored before it have none and use their seller's current rate at each sale |

## Several servers on one database

Point every server at the same MySQL or PostgreSQL database and give each its own `database.serverId`.
A server brings back after a restart only the builds it started itself, so two servers never resume the
same build.

| Shared | Note |
|---|---|
| Crews and warehouses | Stored by player and read when the player joins a server. A crew is written as it changes, a warehouse every 30 seconds and when its owner leaves |
| Build history | Lists a player's builds from every server |
| Trust lists | Read once at startup: a change made on another server is seen after a restart |
| The player market | One market for every server: see [Player market](../features/market/index.md#several-servers) |

## Moving from SQLite to MySQL or PostgreSQL

1. Stop the server and keep a copy of `data/uxmbuilders.db`.
2. Point `database.url` at MySQL or PostgreSQL and start the server. The plugin creates its tables
   there.
3. With nobody online, run `/builders admin migrate sqlite-to-mysql` from the console.

It copies the SQLite file into the live database in one transaction: all of it, or nothing. It refuses
to run while SQLite is still the live database, when `data/uxmbuilders.db` is missing, while players are
online (an online player's crew is held in memory and would be written back over the copy), and when the
target already holds builds, warehouse stock, trust, audit or market rows. The market's tables are copied
with the rest. Player and builder rows written by
players who only joined are replaced. The SQLite file is left untouched as a backup.

## Notes

- `data/` also holds `metadata.yml`, `license.cache`, `instance.id` and `pending-returns.yml`. Delete
  none of them while the server runs.
- `data/market-cache/` keeps a copy of market designs read from the database, so they are not read
  again. It can be deleted while the server is stopped; the database holds the designs.
- Every table has a primary key, so MySQL servers that require one, the default on several managed
  hosts, are supported.

Related: [Restarts and refunds](../features/recovery.md), [config.yml](config-yml.md)
