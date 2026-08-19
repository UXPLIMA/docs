---
title: Online data
order: 266
description: Load a player's data on join, keep it in memory while they play, flush it on a timer, save it on quit.
icon: users
---

The lifecycle every plugin with per-player data writes, and usually writes with at least one bug in
it.

```java
OnlineDataManager<Profile> profiles = new OnlineDataManager<>(
        plugin, scheduler, store, Duration.ofMinutes(5));

profiles.installListener(plugin);   // wires join and quit
profiles.start();                    // begins the periodic flush
```

```java
Profile profile = profiles.get(uuid);
profiles.isLoaded(uuid);
profiles.onlineCount();
profiles.flush();
profiles.stop();                     // in onDisable
```

`handleJoin` and `handleQuit` are exposed too, for a host that already has its own listeners and does
not want a second pair registered.

## The store seam

```java
DataStore<Profile> store = new DataStore<>() {
    @Override public Profile load(UUID id) { return repository.findById(id).orElseGet(Profile::new); }
    @Override public void save(UUID id, Profile value) { repository.save(value); }
};
```

Two blocking methods, keyed by UUID. Point it at a `Repository`, a `WriteBehindStorage`, a flat file,
or an in-memory map in a test.

The seam lives in `uxmlib-integration` rather than depending on `uxmlib-storage`, which is what keeps
the JDBC stack off consumers who do not want it.

## Threading

Every store call runs off the main thread on `scheduler.async`: the load on join, the save on quit,
and the periodic flush. A blocking backend never stalls the server.

The cache is a concurrent map, so the async hand-back of a loaded value and a main-thread `get` are
safe without locking.

Your `DataStore` implementation may block on I/O and must never touch the Bukkit API.

<Callout type="warning" title="get can return null immediately after join">

The load is asynchronous, so a player's value is not there the instant they join. Check `isLoaded`
before acting on `get`, or handle the null.

</Callout>

## Flush policy

The periodic flush saves every currently-online value. It does not consult a dirty flag, so the value
type does not need one, and at worst it writes an unchanged value.

That is deliberately simple and safe. If you want dirty-only flushing, layer it into your `DataStore`:
put a `WriteBehindStorage` behind the seam and the coalescing happens there.

## A load failure

If your `load` throws, the manager routes the failure to its error sink and leaves that player
unloaded rather than caching a broken value or half-loading. `isLoaded` then reports false, which is
the signal to deny an action rather than proceed on defaults.
