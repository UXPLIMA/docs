---
title: uxmlib-item
order: 22
description: A fluent item builder for the 1.21 component API, player heads with async skin resolution, persistent data helpers and component-safe serialization.
icon: package-2
---

Everything about building, describing, storing and identifying an `ItemStack`. Depends on
`uxmlib-common`. The GUI module depends on this one.

The 1.21 item component API changed enough that most builders written before it are wrong in
interesting ways: enchantments and attributes became registry entries, custom model data grew a
component form, and serialization has to survive components it does not know about. This module
targets the current API directly rather than abstracting over the one that came before.

| Page | Covers |
|---|---|
| [ItemBuilder](item-builder.md) | The full builder surface, and items defined in config |
| [Player heads](heads.md) | `SkullData`, async skin resolution, rate limiting |
| [Persistent data](persistent-data.md) | Typed PDC helpers, flags, single-key similarity, action blocking |
| [Serialization](serialization.md) | Component-safe bytes and base64, gzip, data-version migration |

## The one-liner

```java
ItemStack sword = ItemBuilder.of(Material.DIAMOND_SWORD)
        .name(Text.mini("<gradient:#ff5555:#ffaa00>Flameblade</gradient>"))
        .lore(Text.mini("<gray>A legendary weapon"))
        .enchant(Items.enchantment("sharpness"), 5)
        .flags(ItemFlag.HIDE_ENCHANTS)
        .unbreakable(true)
        .build();
```

## Registry lookups

1.21 made enchantments and attributes registry entries, so they are looked up by key rather than
named as constants.

```java
Items.enchantment("sharpness");
Items.enchantment(NamespacedKey.minecraft("mending"));
Items.attribute("generic.max_health");
Items.attribute(key);
```

## Giving items to a player

```java
Items.give(player, sword, shield);
Items.give(scheduler, player, sword);
```

The plain form drops anything that does not fit at the player's feet. The `Scheduler` form does the
same on the player's entity thread, which is what you want when the give is triggered from an async
callback.
