---
title: Persistent data
order: 223
description: Typed PDC access, boolean flags, UUID storage, single-key similarity and blocking item actions.
icon: database
---

Persistent data containers are how an item carries plugin state. The API is verbose enough that most
code wraps it; this is that wrapper.

## Reading and writing

```java
Pdc.set(item, key, PersistentDataType.STRING, "value");

Pdc.has(item, key, PersistentDataType.STRING);
Optional<String> value = Pdc.get(item, key, PersistentDataType.STRING);
String orDefault = Pdc.getOrDefault(item, key, PersistentDataType.STRING, "none");
```

For several reads off one item, take a reader once rather than opening the container per call:

```java
Pdc.PdcReader reader = Pdc.read(item);

reader.has(key, PersistentDataType.STRING);
reader.get(key, PersistentDataType.INTEGER);
reader.getOrDefault("owner", UuidArrayType.INSTANCE, fallback);
```

`Items.editPdc(item, pdc -> ...)` is the raw form when you need the container itself.

## Boolean flags

A flag has no natural persistent type, so it ends up as a byte everybody encodes differently.

```java
PdcFlag.set(pdc, key, true);
PdcFlag.get(view, key);
PdcFlag.getOrDefault(view, key, false);
PdcFlag.has(view, key);
PdcFlag.remove(pdc, key);
```

## UUIDs

```java
Pdc.set(item, ownerKey, UuidArrayType.INSTANCE, player.getUniqueId());

int[] raw = UuidArrayType.toIntArray(uuid);
UUID back = UuidArrayType.fromIntArray(raw);
```

Stored as an int array, which is compact and matches how Minecraft itself stores UUIDs in NBT.

## Single-key similarity

`ItemStack.isSimilar` compares everything. Often you only care whether two items carry the same
plugin identity, and not whether their lore or durability match.

```java
boolean sameToken = Items.isSimilar(a, b, tokenKey, PersistentDataType.STRING);
```

This is what makes a custom currency stack correctly when one copy has been renamed, or a quest item
match after its lore was regenerated.

## Blocking item actions

Some items should not be craftable, edible, placeable or droppable. Stamp the item once and the
listener enforces it.

```java
ItemBlockerType.block(item, ItemAction.CRAFT, ItemAction.DROP);
ItemBlockerType.block(item, Set.of(ItemAction.CONSUME));

ItemBlockerType.isBlocked(item, ItemAction.DROP);
Set<ItemAction> blocked = ItemBlockerType.blockedActions(view);
```

Register the listener once:

```java
Bukkit.getPluginManager().registerEvents(new ItemActionListener(), plugin);
```

| Action | Event cancelled |
|---|---|
| `CRAFT` | `PrepareItemCraftEvent`, the result is suppressed |
| `CONSUME` | `PlayerItemConsumeEvent` |
| `PLACE` | `BlockPlaceEvent` |
| `DROP` | `PlayerDropItemEvent` |

Each action has a stable string id written into the item's data, never the enum name, so the enum can
be reordered or extended without invalidating items already stamped. The key is
`uxmlib:blocked_actions`.
