---
title: uxmlib-storage
order: 25
description: Pooled JDBC with a query builder, versioned migrations, schema tools, caching layers and cross-server row sync.
icon: hard-drive
---

Plain JDBC, pooled by HikariCP, with the pieces around it that a plugin otherwise writes badly:
parameterised queries that cannot be concatenated wrong, migrations that run once, a cache that
matches the shape of player data, and a way to keep several servers in step.

There is no ORM here and no entity manager. A `Repository` is a base class you extend with a row
mapper and a bind, not a framework.

| Page | Covers |
|---|---|
| [Database](database.md) | Pooling, SQLite and network backends, transactions, dialects |
| [Queries](queries.md) | `Sql`, the query builders, resilient loads, keyset paging, typed columns |
| [Migrations](migrations.md) | Versioned migrations, schema introspection, column operations |
| [Caching](caching.md) | Write-through, write-behind, the player profile cache, repositories |
| [Cross-server sync](sync.md) | Row sync, synchronizers, message encoding |

## The short version

```java
Database db = Database.builder().sqlite(dataFolder.resolve("data.db")).build();

new MigrationRunner(db).apply(List.of(
        new Migration(1, "init", "CREATE TABLE players (uuid TEXT PRIMARY KEY, coins INTEGER)")));

Sql sql = new Sql(db);

Query top = SelectBuilder.from("players")
        .where("coins", ">=", 100)
        .orderByDescending("coins")
        .limit(10)
        .build();

List<String> leaders = sql.query(top, row -> row.getString("uuid"));
```

SQLite is the default and needs no configuration. Switching to MySQL, MariaDB, PostgreSQL or H2 is a
JDBC URL, credentials and the matching driver on the classpath.
