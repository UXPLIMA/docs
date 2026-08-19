---
title: uxmlib-redis
order: 30
description: A low-level binary Redis pub/sub bus for fanning an opaque frame across the nodes sharing one Redis.
icon: radio
---

The smallest useful cross-server primitive: publish a `byte[]` to a named channel, subscribe to a
named channel. It owns the wire and nothing else. Encoding, routing and echo suppression are yours.

This module depends on **nothing internal**, not even `uxmlib-common`. Lettuce is a compile-only soft
dependency, so it is not shipped with the module.

```java
RedisBus bus = new LettuceRedisBus(RedisClient.create("redis://localhost:6379"), logger::warning);

bus.subscribe("party-updates", frame -> applyUpdate(decode(frame)));
bus.publish("party-updates", encode(update));

bus.healthy();
bus.close();
```

## What it guarantees

**Publish never blocks.** It is fire-and-forget over the client's event loop and may be called from
any thread.

**Publish fails degraded.** A publish while the connection is down is dropped and warned about, not
thrown. A Redis outage degrades cross-server messaging; it does not take the server with it.

**Subscriptions reconnect.** Each subscription re-establishes itself after a connection drop, so a
node that lost Redis for thirty seconds resumes receiving without a restart.

**Warnings are rate-limited.** A connection that is down produces one warning periodically rather
than one per publish.

## What you have to do

**Bridge threads yourself.** `subscribe` delivers frames on the client's pub/sub thread. Nothing
Bukkit may be touched there. Hop through the `Scheduler` first.

```java
bus.subscribe("party-updates", frame -> scheduler.global(() -> apply(decode(frame))));
```

**Suppress your own echo.** Redis pub/sub delivers a publisher its own messages. A node that both
publishes and subscribes on a channel will receive what it just sent, so stamp your frames with a
node id and drop your own.

**Choose an encoding.** The frame is opaque bytes. Protobuf, your own binary format, or a UTF-8
string, whichever fits.

## When to use this rather than storage sync

| Need | Use |
|---|---|
| An opaque binary payload, no relational dependency | `RedisBus` |
| A small text id or blob tied to the storage stack | `DataSynchronizer` in [`uxmlib-storage`](storage/sync.md) |
| Eventual consistency with no extra infrastructure | Row sync in `uxmlib-storage` |

`uxmlib-storage` builds its `DataSynchronizer` on the same idea with a `String` payload and a
`LocalDataSynchronizer` fallback for single-node servers. Reach for `uxmlib-redis` when the payload is
a codec's output rather than an identifier, or when you want the primitive with no other module
attached.
