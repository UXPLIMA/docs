---
title: uxmlib-storage
order: 25
icon: database
---

Plain JDBC with a connection pool, a query builder that cannot be injected into, versioned
migrations, caches, and cross-server row sync. No ORM.

## A database

```java
Database db = Database.builder()
        .sqlite(dataFolder.resolve("data.db"))
        .build();

Sql sql = new Sql(db);
sql.execute("CREATE TABLE IF NOT EXISTS players (uuid TEXT PRIMARY KEY, coins INTEGER)");
```

SQLite is the default and is configured with WAL, which is the setting that makes concurrent reads
work properly.

For a network backend, give the builder a JDBC URL and credentials and add the driver:

```java
Database db = Database.builder()
        .jdbcUrl("jdbc:mariadb://localhost:3306/plugin")
        .username("user")
        .password("secret")
        .build();
```

MariaDB, MySQL, PostgreSQL and H2 are opt-in — the driver is yours to add, so a plugin using SQLite
does not ship four JDBC drivers it never loads.

Pooling is HikariCP.

## Migrations

```java
new MigrationRunner(db).apply(List.of(
        new Migration(1, "init", "CREATE TABLE warps (name TEXT PRIMARY KEY)"),
        new Migration(2, "add-owner", "ALTER TABLE warps ADD COLUMN owner TEXT")));
```

Each runs exactly once, in version order, and is recorded. Leave the list in `onEnable` and append to
it — that is the whole schema history, in code, and a server three versions behind catches up in
order on the next start.

`MigrationFile` and `MigrationResources` load them from resources instead when the SQL is long
enough to deserve its own file.

## Queries

```java
Query top = SelectBuilder.from("players")
        .where("coins", ">=", 100)
        .orderByDescending("coins")
        .limit(10)
        .build();

List<String> leaders = sql.query(top, row -> row.getString("uuid"));
```

Values are bound parameters, never concatenated. `SqlIdentifiers` validates table and column names,
so a name from configuration cannot become SQL either.

`InsertBuilder`, `UpdateBuilder` and `DeleteBuilder` are the same idea for the other statements, and
`TxSql` runs a block in a transaction.

`KeysetPager` pages by key rather than by `OFFSET`, which stays fast on a large table where offset
paging does not.

## Repositories and caches

```java
Repository<UUID, Profile> repo = ...;
CachedStorage<UUID, Profile> cached = new CachedStorage<>(repo, ...);
WriteBehindStorage<UUID, Profile> writeBehind = new WriteBehindStorage<>(repo, scheduler, ...);
```

`CachedStorage` is write-through — a write goes to the database and the cache together. Nothing can
be lost, and every write costs a round trip.

`WriteBehindStorage` batches writes and flushes them. Much cheaper for something written constantly,
at the cost of a window in which a crash loses the last batch. Use it for statistics and playtime;
not for money.

Caching is Caffeine, so entries expire and the cache is bounded.

`PlayerProfileCache` is a two-tier cache built for the specific shape of player data: online players
held, offline players expiring.

## Cross-server sync

```java
DataSynchronizer sync = new LocalDataSynchronizer();
// or, with Redis configured:
DataSynchronizer sync = new RedisDataSynchronizer(bus, ...);
```

A cache on two servers reading one database will each hold a stale copy. `DataSynchronizer`
announces invalidations instead of leaving them to be discovered.

`LocalDataSynchronizer` is the single-node default and does nothing across servers, so the same code
runs in both shapes. `RedisDataSynchronizer` bridges it over
[`uxmlib-redis`](redis.md).

`RowSyncService`, `RowSyncPoller` and `ChangedRow` are the row-level version: a table of changes,
polled and applied, for data that must converge rather than merely be invalidated.

<Callout type="warning" title="Two servers, one database, no sync">

A shared database without a synchronizer is the configuration that silently corrupts data: both
servers hold a cached copy, both write, and the last write wins with no record that anything was
lost. Configure the sync at the same time as the shared database, not later.

</Callout>

## File storage

`FileStorageProvider` is a `StorageProvider` backed by files rather than JDBC, for a plugin small
enough not to want a database at all. Same interface, so moving to one later is a builder change.
