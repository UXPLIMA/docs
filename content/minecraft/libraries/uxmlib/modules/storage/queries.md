---
title: Queries
order: 252
description: Prepared statements without the boilerplate, injection-safe builders, resilient loads, keyset paging and typed columns.
icon: search
---

## Sql

`new Sql(database)` wraps the borrow, prepare, bind, iterate and close dance. Every method uses
prepared statements and try-with-resources, borrows a fresh pooled connection, and wraps checked
`SQLException`s in `StorageException`.

```java
Sql sql = new Sql(db);

List<Warp> warps = sql.query(
        "SELECT * FROM warps WHERE world = ?",
        st -> st.setString(1, world),
        row -> new Warp(row.getString("name"), row.getString("world")));

Optional<Warp> one = sql.queryFirst(
        "SELECT * FROM warps WHERE name = ?",
        st -> st.setString(1, name),
        Warp::from);

int changed = sql.update("DELETE FROM warps WHERE name = ?", st -> st.setString(1, name));

sql.execute("CREATE TABLE IF NOT EXISTS warps (name TEXT PRIMARY KEY)");

long id = sql.insertReturningKey("INSERT INTO logs (message) VALUES (?)", st -> st.setString(1, msg));

int[] results = sql.batch("INSERT INTO logs (message) VALUES (?)", binders);
```

There is no method anywhere that takes a concatenated string of values. Parameters are bound, always.

### Async

```java
CompletableFuture<List<Warp>> future = sql.queryAsync(executor, sql, binder, mapper);
CompletableFuture<Integer> updated = sql.updateAsync(executor, sql, binder);
```

You supply the executor, so the module never owns a thread pool you did not ask for. In a plugin this
is usually backed by `scheduler.async`.

## Resilient loads

One malformed row should not lose the whole table.

```java
LoadResult<Warp> result = sql.queryResilient(
        "SELECT * FROM warps", st -> {}, Warp::from);

result.rows();        // everything that mapped
result.skipped();     // how many did not
result.hadSkips();
```

A mapper throwing on a single row logs and skips that row. A failure of the query itself still aborts
with a `StorageException`, because that is not recoverable.

This is the right default for loading operator-visible data, where one hand-edited row should not
take out the other four hundred.

## Streaming a large table

```java
sql.forEachByKey("players", "uuid", 500, Player::from, player -> migrate(player));
```

Keyset pagination: each page is fetched with `WHERE key > ? ORDER BY key LIMIT n` and the cursor
advances to the last key of the page. No `OFFSET`, which degrades on large tables.

The page is buffered and its connection returned to the pool **before** your consumer runs, so a
multi-thousand-row walk holds no connection across the callback. The callback may itself query the
database.

<Callout type="warning" title="The key column must be strictly unique">

The cursor advances with `key > ?`, so rows sharing the last key of a page are skipped silently. Pass
a primary or unique key. This is not validated for you.

</Callout>

## The query builders

Injection-safe by construction: values become bound parameters, never text.

```java
Query top = SelectBuilder.from("players")
        .columns("uuid", "coins")
        .where("world", "spawn")
        .where("coins", ">=", 100)
        .whereIn("rank", "vip", "mvp")
        .whereLikeIgnoreCase("name", "steve%")
        .whereAny(or -> or.eq("rank", "vip").eq("staff", true))
        .orderByDescending("coins")
        .limit(10)
        .offset(20)
        .build();

List<String> leaders = sql.query(top, row -> row.getString("uuid"));
```

```java
Query insert = InsertBuilder.into("warps")
        .set("name", name)
        .set("world", world)
        .build();

Query update = UpdateBuilder.table("players")
        .set("coins", 100)
        .where("uuid", uuid)
        .build();

Query delete = DeleteBuilder.from("warps")
        .where("name", name)
        .build();

sql.update(insert);
```

A `Query` is a record of the SQL text and its parameter list, so it can be built in one place,
inspected or logged, and executed in another.

`whereAny` builds an OR group, which is the one thing a flat `where` chain cannot express.

## Typed columns

`SqlType<T>` pairs a JDBC type with encode and decode functions, so a column's representation is
declared once.

```java
SqlType<UUID> id = SqlType.uuid();
SqlType<Instant> when = SqlType.instant();
SqlType<Component> label = SqlType.component();

id.bind(statement, 1, uuid);
UUID read = id.read(row, "uuid");
```

Built in: `text`, `bigint`, `integer`, `real`, `bool`, `uuid`, `instant`, `component`.

`andThen(decode, encode, readType)` derives a new type from an existing one, so a domain wrapper over
a string costs one line rather than a serializer.

## Codecs

```java
int mask = Codecs.enumSetToBitmask(EnumSet.of(Flag.PVP, Flag.FIRE));
EnumSet<Flag> flags = Codecs.bitmaskToEnumSet(mask, Flag.class);

int[] parts = Codecs.uuidToIntArray(uuid);
UUID back = Codecs.uuidFromIntArray(parts);

Codecs.ByteForm form = Codecs.detectUuidBytes(bytes);
```

`detectUuidBytes` matters when adopting a table another plugin wrote: UUIDs live in databases as
16-byte arrays, as 36-character text and as 32-character hex, and this tells you which one you are
looking at.

## Results

`Result<T>` is a small ok-or-error value for operations where a failure is expected rather than
exceptional.

```java
Result<Warp> result = loadWarp(name);

if (result.isOk()) use(result.value());
Warp warp = result.orElse(defaultWarp);
Result<String> mapped = result.map(Warp::name);
```
