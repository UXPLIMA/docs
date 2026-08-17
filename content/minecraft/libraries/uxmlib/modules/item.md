---
title: uxmlib-item
order: 22
description: ItemBuilder, player heads, component-safe serialization and persistent data.
icon: sword
---

A fluent item builder, player heads, serialization that survives data components, and typed
persistent data.

## ItemBuilder

```java
ItemStack sword = ItemBuilder.of(Material.DIAMOND_SWORD)
        .name(Text.mini("<gradient:#ff5555:#ffaa00>Flameblade</gradient>"))
        .lore(Text.mini("<gray>A legendary weapon"))
        .enchant(Items.enchantment("sharpness"), 5)
        .flags(ItemFlag.HIDE_ENCHANTS)
        .unbreakable(true)
        .build();
```

Covers names, lore, enchantments, attributes, flags, durability, banners and data components — each
with a matching remover, so a builder can take an existing stack apart as well as put one together.

### Registry lookups

1.21 turned enchantments and attributes into registry entries rather than enums. `Items.enchantment`
and its siblings do the lookup by key:

```java
.enchant(Items.enchantment("sharpness"), 5)
```

Writing `Enchantment.DAMAGE_ALL` still compiles on some versions and is the API that keeps moving.
Going through the registry by key is the form that survives point releases.

## Player heads

```java
ItemStack head = ItemBuilder.of(Material.PLAYER_HEAD)
        .skull(SkullData.ofName("Notch"))
        .build();
```

`SkullData` is a sealed type — by name, by UUID, or by base64 texture. The skin resolver is async:
resolving a name to a texture is an HTTP call to Mojang, and the builder never blocks the main thread
to make one.

`SkullResolver` caches results, and `PaperProfileCompleter` fills in a profile when Paper can do it
locally.

## Serialization

```java
String saved = ItemSerialization.toBase64(sword);
ItemStack back = ItemSerialization.fromBase64(saved);
```

Component-safe: it survives every data component, which the naive
`YamlConfiguration`-round-trip does not on 1.21. A gzip variant is available for storing many stacks.

Use this for anything that has to be an exact stack later — a vault, a kit, a claimed reward. Use
`ItemConfig` instead when the item should be editable by the server owner.

## ItemConfig

Items defined in configuration rather than code: material, name, lore, enchantments, flags, skull
data. The same shape the rest of the library reads, so an operator can re-skin a menu without
touching a plugin.

## Persistent data

```java
Pdc.set(item, key, PdcTypes.UUID_ARRAY, ids);
Optional<UUID[]> ids = Pdc.get(item, key, PdcTypes.UUID_ARRAY);
```

Typed helpers over the persistent data container, including a `UUID` array type the vanilla API does
not provide, and `PdcFlag` for the common case of a marker with no payload.

## isSimilar by key

Comparing two stacks with the vanilla `isSimilar` compares everything. `Items` offers a single-key
comparison instead — two stacks are "the same item" if they carry the same value under one PDC key,
whatever else differs.

That is what you want for a shop or a quest: the same reward item after a lore change is still the
same item.

## Item actions

`ItemAction` and `ItemActionListener` drive behaviour from an item's persistent data — a right-click
that runs a command, gated by a `RateLimiter` and an `ItemBlockerType`.

These are the listeners the standalone `uxmlib-all` jar registers. Everything else in the library is
consumed as an API; this is the one part that acts on its own.
