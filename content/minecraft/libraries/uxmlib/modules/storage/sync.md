---
title: Cross-server sync
order: 255
description: "Keeping several servers in step: row polling with a version cursor, and a pluggable message bus."
icon: network
---

Two servers sharing one database will happily serve stale data to each other. There are two problems
in that sentence: noticing a row changed, and telling the other node about it.

## Row sync

Polls a table for rows whose version column moved past a cursor, and hands them to a listener.

```java
RowSyncConfig config = RowSyncConfig.builder("players", "uuid", "version", "updated_by", nodeId)
        .batchLimit(200)
        .startCursor(0)
        .build();

RowSyncPoller<Profile> poller = new RowSyncPoller<>(sql, config, Profile::from, changed -> apply(changed));

RowSyncService<Profile> service = new RowSyncService<>(poller);
service.start(scheduler, Duration.ofSeconds(5));
service.stop();
service.isRunning();
```

| Column | Purpose |
|---|---|
| Key column | Identifies the row |
| Version column | A monotonically increasing value bumped on every write |
| Updated-by column | Which node wrote it |

The updated-by column is what stops a node reacting to its own writes: rows stamped with your own
node id are skipped.

```java
poller.skipWhenDirty(key -> writeBehind.isDirty(key));
poller.cursor();
int applied = poller.pollOnce();
```

`skipWhenDirty` is the important one. A row you have unsaved local edits for should not be
overwritten by a poll; skipping it lets your flush win rather than resurrecting stale data.

`pollOnce` runs a single pass, which is what a test drives and what an admin command can trigger.

## The message bus

Row sync answers "what changed". A synchronizer answers "tell the others now".

```java
DataSynchronizer sync = new LocalDataSynchronizer();
DataSynchronizer sync = new RedisDataSynchronizer("redis://localhost:6379");
DataSynchronizer sync = new RedisDataSynchronizer(uri, "myplugin");   // channel namespace

Subscription subscription = sync.subscribe("profiles", payload -> invalidate(payload));
sync.publish("profiles", uuid.toString());

subscription.close();
sync.close();
```

`LocalDataSynchronizer` is the single-node default and delivers in-process. `RedisDataSynchronizer`
bridges the same contract across nodes. Code written against `DataSynchronizer` does not change when
a server grows from one node to five.

The umbrella parameter namespaces channels, so two plugins on one Redis do not collide.

## Message encoding

```java
String frame = SyncMessage.encode(new SyncMessage("profiles", payload));
SyncMessage message = SyncMessage.decode(frame);
```

A tiny channel-plus-payload envelope, so the transport carries a string and the routing stays
explicit.

## Which to use

| Need | Use |
|---|---|
| Eventual consistency, no extra infrastructure | Row sync alone |
| Immediate invalidation across nodes | A synchronizer |
| Both | Publish on write, poll as the backstop |

Row sync alone is often enough and needs nothing but the database you already have. Add a
synchronizer when a five-second poll interval is too long to be correct.

For a lower-level binary bus with no relational dependency at all, see
[`uxmlib-redis`](../redis.md).
