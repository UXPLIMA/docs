---
title: ItemBuilder
order: 221
description: Every method on the builder, grouped by what it touches, plus items defined in HOCON.
icon: hammer
---

```java
ItemBuilder.of(Material.DIAMOND);      // a fresh item
ItemBuilder.from(existingStack);       // edit a copy of an existing one
```

Every method returns the builder. `build()` produces the `ItemStack`.

## Identity and amount

| Method | Effect |
|---|---|
| `material(Material)` | Change the material, keeping the meta |
| `amount(int)` | Stack size |
| `maxStackSize(int)` | Override the maximum stack size |
| `rarity(ItemRarity)` | The rarity colour of the name |
| `damage(int)` | Durability damage |
| `unbreakable(boolean)` | Never loses durability |
| `enchantable(int)` | The enchantability value |

## Name and lore

```java
ItemBuilder.of(Material.PAPER)
        .name(Text.mini("<gold>Deed"))
        .lore(Text.mini("<gray>Line one"), Text.mini("<gray>Line two"))
        .addLore(Text.mini("<dark_gray>Appended"))
        .build();
```

`lore(List<Component>)` takes a list. `clearName()` and `clearLore()` remove them, which matters when
you are editing an existing item rather than building a fresh one.

`Lore.wrap(line, maxWidth)` splits a long line into several at word boundaries, so a description read
from config does not run off the tooltip.

## Enchantments and flags

```java
.enchant(Items.enchantment("sharpness"), 5)
.removeEnchant(Items.enchantment("sharpness"))
.clearEnchants()
.storedEnchant(Items.enchantment("mending"), 1)   // for an enchanted book
.flags(ItemFlag.HIDE_ENCHANTS, ItemFlag.HIDE_ATTRIBUTES)
.removeFlags(ItemFlag.HIDE_ENCHANTS)
.glow(true)                                        // the enchant shimmer, no real enchantment
```

`glow(true)` is the trick every plugin reimplements: an enchantment plus a hide flag, done for you.

## Attributes

```java
.attribute(Items.attribute("generic.attack_damage"),
        new AttributeModifier(key, 4.0, AttributeModifier.Operation.ADD_NUMBER))
```

## Model and tooltip

```java
.customModelData(42)
.customModelData(component)               // the 1.21.4 component form
.customModelDataFloats(List.of(1f, 0.5f))
.itemModel(NamespacedKey.minecraft("custom/sword"))
.tooltipStyle(NamespacedKey.minecraft("fancy"))
.hideTooltip(true)
```

## Type-specific

| Item type | Methods |
|---|---|
| Player head | `skull(SkullData)` |
| Potion | `potionEffect(PotionEffect)`, `potionColor(Color)` |
| Firework | `fireworkEffect(FireworkEffect)`, `fireworkPower(int)` |
| Leather armour | `leatherColor(Color)` |
| Book | `bookTitle`, `bookAuthor`, `bookPages` |
| Banner | `bannerPattern(DyeColor, PatternType)`, `bannerPattern(Pattern)`, `bannerPatterns(List<Pattern>)` |
| Map | `mapColor`, `mapScaling`, `mapView`, `mapLocationName` |

## Escape hatches

```java
.editMeta(meta -> meta.setCustomModelData(1))
.editTypedMeta(SkullMeta.class, meta -> meta.setNoteBlockSound(key))
.editPersistentData(pdc -> pdc.set(key, PersistentDataType.STRING, "value"))
```

`editTypedMeta` is the safe form: it only runs when the item's meta is actually that type, so a
misapplied edit is a no-op rather than a `ClassCastException`.

## Items from config

The single most-replicated pattern in every plugin, done once here. It is also the seam the GUI menu
loader sits on.

```hocon
material = DIAMOND_SWORD
name = "<red>Blade"
lore = ["<gray>line one", "line two"]
amount = 1
enchants { sharpness = 5 }
flags = [HIDE_ENCHANTS]
custom-model-data = 42
unbreakable = true
glow = true
skull = "Notch"
```

```java
ItemStack item = ItemConfig.load(node).build();
ItemStack greeted = ItemConfig.load(node, List.of(Text.placeholder("player", name))).build();
ItemStack wrapped = ItemConfig.load(node, resolvers, 40).build();   // auto-wrap lore at 40
```

Name and lore pass through MiniMessage with whatever resolvers you supply. A `\n` inside a lore entry
splits it into several lines. `skull` is only read for `PLAYER_HEAD`.

The third parameter is a wrap width, so an operator can write one long description line and get a
tidy tooltip.

<Callout type="note" title="ItemConfig is one-way">

Config to item. There is no writer yet, so an item edited in game is not persisted back to the file.
The in-game config editor in the GUI module edits config values, not items.

</Callout>
