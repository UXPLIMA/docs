---
title: Database
order: 251
description: The pooled connection source, SQLite defaults, network backends, transactions and dialect handling.
icon: server
---

## From a configuration block

Most plugins should not build a URL at all. `StorageSettings` reads one named block and opens the
store it describes:

```hocon
storage {
  type         = "sqlite"      # sqlite, mysql, mariadb or postgresql
  file         = "data.db"     # the SQLite file, inside the plugin folder
  host         = "localhost"
  port         = 0             # 0 is the engine's own: 3306, or 5432 for postgresql
  database     = "minecraft"
  username     = "minecraft"
  password     = ""
  table-prefix = "myplugin_"
  pool-size    = 4
}
```

```java
StorageSettings storage = StorageSettings.read(config, "storage", "data.db", "myplugin_");
Database db = storage.open(dataFolder, "myplugin");
storage.migrate(db, migrations, "profile");     // {prefix} filled in, history kept under the prefix
String table = storage.table("profile");        // "myplugin_profile"
```

A type nobody recognises stops the start instead of falling back to a file, because a server that
believed it was on a network database and quietly wrote to SQLite splits the network in two.

`migrate` writes `{prefix}` into every migration, keeps the history in `<prefix>schema_history`, and
carries over what the shared `uxmlib_schema_history` already recorded for this schema, so a plugin that
moves onto its own history runs nothing twice.

### Drivers

The server carries the MySQL and SQLite drivers. `StorageDrivers.coordinates()` names the MariaDB and
PostgreSQL ones, for a Paper `PluginLoader` to fetch with `MavenLibraryResolver` when the server starts,
so a plugin jar carries none of them.

### Testing on every backend

`uxmlib-storage-testing` runs a test once on each engine, on servers it starts in containers:

```java
@EveryBackend
void aProfileComesBack(RealDatabase real) {
    Database db = real.open();
    real.settings().migrate(db, migrations, "profile");
    // write through one store, read back through a new one
}
```

The tests carry the tag `real-database`, so a build can run them in a task of their own. SQLite
accepts almost anything, and a store proved only on it is a store nobody has seen work on the others.

## Building one by hand

```java
Database db = Database.builder()
        .sqlite(dataFolder.resolve("data.db"))
        .build();
```

```java
Database db = Database.builder()
        .jdbcUrl("jdbc:mysql://localhost:3306/server")
        .username("plugin")
        .password(secret)
        .maxPoolSize(10)
        .connectionTimeoutMs(5_000)
        .poolName("myplugin")
        .build();
```

| Method | Sets |
|---|---|
| `sqlite(Path)` | A file-backed SQLite database |
| `sqliteInMemory()` | An in-memory SQLite database, for tests |
| `jdbcUrl(String)` | Any JDBC URL |
| `username` / `password` | Credentials for a network backend |
| `maxPoolSize(int)` | Pool size |
| `connectionTimeoutMs(long)` | How long a borrow waits before failing |
| `busyTimeoutMs(int)` | SQLite busy timeout |
| `journalMode(JournalMode)` | SQLite journal mode; WAL by default |
| `poolName(String)` | The pool name in logs and metrics |

`Database.adopt(dataSource, dialect)` wraps a `DataSource` you already have, for a host that manages
pooling itself.

<Callout type="tip" title="SQLite defaults to WAL">

Write-ahead logging lets readers and a writer proceed concurrently, which is what makes SQLite viable
for a plugin that reads on the main thread and writes from async tasks. Change it only if you know
why.

</Callout>

## Using it

```java
db.ping();          // is the backend reachable
db.isClosed();
db.dialect();
db.dataSource();
db.close();         // in onDisable
```

`Database` is `AutoCloseable`. Close it on disable so the pool's threads go away with your plugin.

## Transactions

```java
int rowsAffected = db.transaction(tx -> {
    tx.update("UPDATE accounts SET coins = coins - ? WHERE uuid = ?", st -> {
        st.setInt(1, amount);
        st.setString(2, from);
    });
    return tx.update("UPDATE accounts SET coins = coins + ? WHERE uuid = ?", st -> {
        st.setInt(1, amount);
        st.setString(2, to);
    });
});
```

```java
db.inTransaction(tx -> {
    tx.execute("DELETE FROM sessions WHERE expired = 1");
});
```

The block runs against a `TxSql` bound to one connection, so every statement in it is part of the
same transaction. Returning normally commits; throwing rolls back.

`TxSql` offers `query`, `queryFirst`, `update` and `execute`. It deliberately has no async methods:
a transaction is one connection on one thread.

## Dialects

```java
Dialect dialect = db.dialect();
Dialect inferred = Dialect.fromJdbcUrl(url);
```

`SQLITE`, `MYSQL`, `POSTGRES`, `H2`, `GENERIC`. Inferred from the JDBC URL prefix.

It exists because the dialects genuinely diverge where it matters most, on insert-or-update:

```java
String sql = dialect.upsert("players", "uuid", List.of("coins", "level"));
```

SQLite and Postgres spell it `ON CONFLICT`, MySQL and MariaDB `ON DUPLICATE KEY`, H2
`MERGE INTO ... KEY(...)`. Writing that by hand is how a plugin ends up working on one backend only.

`dialect.alterColumnType(table, column, type)` is the same idea for a column type change.
