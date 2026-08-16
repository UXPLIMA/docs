---
title: Events
order: 870
description: uxmSkyblock fires standard Bukkit events at key island lifecycle moments,
  so your plugin or module can react to them.
---

---

## Available Events

| Event | Fired when |
|-------|------------|
| `net.cengiz1.uxmskyblock.event.IslandCreateEvent` | An island finishes being created |
| `net.cengiz1.uxmskyblock.event.IslandDeleteEvent` | An island is deleted |

`IslandCreateEvent` fires at the end of creation (after the island is built and
finalized); `IslandDeleteEvent` fires during deletion.

---

## Listening for Them

These are ordinary Bukkit events: register a listener as usual:

```java
public class MyListener implements Listener {

    @EventHandler
    public void onIslandCreate(IslandCreateEvent event) {
        Island island = event.getIsland();
        // e.g. give the owner a starter kit, set up your add-on's data, ...
    }

    @EventHandler
    public void onIslandDelete(IslandDeleteEvent event) {
        Island island = event.getIsland();
        // clean up any data your plugin/module stored for this island
    }
}
```

Register it from your plugin's `onEnable`:

```java
getServer().getPluginManager().registerEvents(new MyListener(), this);
```

…or, from a module, via the `ModuleContext` (so it's cleaned up automatically on
disable): see [Writing a Module](modules.md).

---

## How Chunklock Uses Them

The [Chunklock](../../chunklock/overview.md) module listens to both:

- On **create**, it sets up the chunk-lock layer for the new island and applies the
  starting open area.
- On **delete**, it clears the island's stored chunk-lock data.

This is exactly the pattern your own add-on should follow.
