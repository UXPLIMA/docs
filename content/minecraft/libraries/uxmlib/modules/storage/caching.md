---
title: Caching and repositories
order: 254
description: A CRUD base class, write-through and write-behind caches, and the player-lifecycle profile cache.
icon: layers
---

## StorageProvider

The seam everything here composes over: `findById`, `findAll`, `save`, `deleteById`.

Two implementations ship. `Repository` is the SQL one. `FileStorageProvider` writes one HOCON file
per entity in a directory, which is a reasonable backend for a small set of operator-edited records.

Because the caches take a `StorageProvider`, a test can supply an in-memory one and the caching
behaviour is testable with no database at all.

## Repository

CRUD by id over one table, without rewriting the same five statements.

```java
final class WarpRepository extends Repository<String, Warp> {

    WarpRepository(Sql sql) {
        super(sql, "warps", "name", List.of("name", "world", "x", "y", "z"),
                row -> new Warp(row.getString("name"), row.getString("world"), ...));
    }

    @Override
    protected void bind(PreparedStatement statement, Warp warp) throws SQLException {
        statement.setString(1, warp.name());
        statement.setString(2, warp.world());
        ...
    }
}
```

```java
Optional<Warp> warp = warps.findById("spawn");
List<Warp> all = warps.findAll();
boolean exists = warps.exists("spawn");
warps.save(warp);            // an upsert, using the dialect's own syntax
warps.deleteById("spawn");
```

The columns list is every column `bind` writes, in order, including the id.

<Callout type="warning" title="Repository reads and writes block">

It is plain JDBC on the calling thread. Wrap it off-thread for anything on a hot path, or put a cache
in front of it.

</Callout>

## Choosing a cache

| Situation | Use |
|---|---|
| Hot rows, every write must reach the database | `CachedStorage` |
| Hot rows written many times between persists | `WriteBehindStorage` |
| Player profiles, permanent while online and TTL after quit | `PlayerProfileCache` |
| A generic bounded cache of anything | `Cache` |

## Write-through

Reads populate the map on a miss; writes go to the backend and then update the cache. The database
stays the source of truth.

```java
CachedStorage<UUID, Profile> profiles = new CachedStorage<>(repository, Profile::id);

profiles.load(uuid);                    // pin on join
Optional<Profile> p = profiles.get(uuid);
profiles.save(profile);
profiles.saveAsync(executor, profile);
profiles.saveAll();
profiles.saveAndInvalidate(uuid);       // on quit
profiles.loadedCount();
```

## Write-behind

A write marks the key dirty and updates memory. Nothing reaches the backend until a flush, so N
writes to one key between flushes collapse into one save, last write wins.

```java
WriteBehindStorage<UUID, Stats> stats = WriteBehindStorage.builder(repository, Stats::id)
        .maximumSize(1000)
        .expireAfterAccess(Duration.ofMinutes(10))
        .build();

stats.save(updated);          // dirty only
stats.flush(uuid);
stats.flushAll();
stats.dirtyCount();
stats.flushAndInvalidate(uuid);
```

This is the path for data that changes many times per tick, like combat statistics, where one batched
persist is far cheaper than one write per edit.

Pending dirty values live in a separate buffer that eviction never touches, so an unsaved write
cannot be lost to a size cap.

Scheduling the periodic flush is deliberately yours. The class stays Paper-free and persists only
when asked.

## The player profile cache

The policy a server actually wants: while a player is online their profile is pinned and never read
through again nor evicted; when they quit it is demoted into a TTL tier that drops it once an idle
window passes.

```java
PlayerProfileCache<UUID, Profile> profiles = PlayerProfileCache.<UUID, Profile>builder()
        .ttlAfterQuit(Duration.ofMinutes(5))
        .maximumOffline(500)
        .build(repository, Profile::id);
```

```java
profiles.markOnline(uuid);     // on join: promote or load
profiles.markOffline(uuid);    // on quit: demote to the TTL tier
Optional<Profile> p = profiles.get(uuid);
profiles.put(profile);
profiles.isOnline(uuid);
profiles.onlineCount();
```

Transitions are driven explicitly from your join and quit handlers, so the cache depends on neither
the scheduler nor the server and is testable on its own.

An online player never re-hits the database. A player who left does not linger in memory forever. A
player who rejoins within the window is a memory hit.

## The generic cache

```java
Cache<String, Warp> cache = Cache.builder()
        .maximumSize(500)
        .expireAfterWrite(Duration.ofMinutes(10))
        .expireAfterAccess(Duration.ofMinutes(2))
        .build();

cache.get(key, k -> load(k));
cache.getIfPresent(key);
cache.put(key, value);
cache.invalidate(key);
cache.invalidateAll();
cache.estimatedSize();
cache.cleanUp();
```

Caffeine-backed, with an injectable `ticker` so expiry is testable without sleeping.
