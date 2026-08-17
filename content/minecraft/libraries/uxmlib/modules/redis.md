---
title: uxmlib-redis
order: 30
icon: radio
---

A binary publish/subscribe bus for fanning a message across the servers sharing one Redis. It is
deliberately small: opaque `byte[]` frames, no relational dependencies, and nothing that knows what
a message means.

```java
bus.subscribe("party-updates", frame -> applyUpdate(frame));
bus.publish("party-updates", encode(update));
```

## Why bytes

The bus does not serialize for you. Encoding is yours — protobuf, JSON, a hand-rolled format — and
the bus moves the bytes.

That is the right boundary for a primitive: a bus with an opinion about serialization forces every
consumer to share it, and a version mismatch between two servers becomes the bus's problem rather
than the message's.

## Failure behaviour

**Publishing is fail-degraded.** With Redis unreachable, `publish` does not throw — the message is
lost and the plugin keeps running. A cross-server notification is not worth taking a server down
for.

**Subscriptions auto-reconnect,** per subscription. A Redis restart does not require a server
restart, and each subscription recovers independently.

`RateLimitedWarner` bounds the logging: an unreachable Redis warns once and then goes quiet rather
than writing a line per publish.

## Lettuce is compile-only

`LettuceRedisBus` is the implementation, and Lettuce is a compile-only soft dependency. You add it
yourself:

```kotlin
implementation("com.github.UXPLIMA.uxmLib:uxmlib-redis:VERSION")
implementation("io.lettuce:lettuce-core:VERSION")
```

A plugin that shades `uxmlib-redis` without using it does not carry a Redis client.

## No internal dependencies

`uxmlib-redis` depends on nothing else in the library — not even `uxmlib-common`. It is usable on its
own, and `uxmlib-storage`'s `RedisDataSynchronizer` builds cache invalidation on top of it without
the storage stack and the bus knowing about each other.

## When you need it

Only when more than one server node has to agree about something: a party that spans servers, a
cache to invalidate, a broadcast to carry.

A single server does not need Redis. `uxmlib-storage` ships a `LocalDataSynchronizer` that
implements the same interface as a no-op, so the same code runs in both shapes and adding Redis later
is a wiring change.
