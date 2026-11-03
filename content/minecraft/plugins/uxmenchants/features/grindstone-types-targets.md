---
title: Grindstone, types and targets
order: 308
description: What a grindstone takes off, how a type draws and caps an enchantment, targets and slots, and every trigger.
icon: shapes
---

## The grindstone

`grindstone.enabled` decides whether ours come off at all. Which of ours come off is the type's
`grindstone-removable`, so a curse stays on because its type says so. The item comes out with our data consistent with
what a player then reads on it.

## Types

```hocon
types {
  normal { name = "@content.types.types.normal.name", order = 1, format = "<value><name> <roman>", limit-per-item = -1, grindstone-removable = true }
  curse  { name = "@content.types.types.curse.name",  order = 2, format = "<bad><name> <roman>",   limit-per-item = 2,  grindstone-removable = false }
}
```

| Key | Meaning |
|---|---|
| `name` | What a player reads for the type: Enchantment, Curse |
| `order` | Where it sorts in a lore block and a window; lowest first |
| `format` | The line an enchantment of this type is drawn as: `<name>`, `<roman>`, `<number>` |
| `limit-per-item` | How many of this type one item may carry; `-1` for no limit |
| `grindstone-removable` | Whether a grindstone takes it off |

Do not write `<level>` in a format: that is a colour role of `theme.conf` and the palette would eat it. Colours are roles,
never hex codes. A second kind of curse, or removable curses, is an edit to this file.

## Targets and slots

A target is a list of items and the slot those items are used from.

| Slot | Fires while the item is |
|---|---|
| `mainhand`, `offhand` | In that hand |
| `hands` | In either hand |
| `helmet`, `chestplate`, `leggings`, `boots` | Worn there |
| `armor` | Worn in any armour slot |
| `any` | Anywhere it is equipped |

The shipped targets: `sword` (with the mace), `axe`, `pickaxe`, `shovel` from the main hand; `bow` (bow and crossbow)
from either hand; `helmet` (with the turtle shell), `chestplate` (with the elytra), `leggings`, `boots` from their slot;
`armor` from any armour slot. The item lists are yours, which is why a bow enchantment reaches a crossbow without a
second enchantment. See [targets.conf](../config/targets-conf.md).

## Triggers

| Trigger | Fires when |
|---|---|
| `attack` | The wearer hits an entity |
| `defend` | An entity hits the wearer |
| `kill` | The wearer kills an entity |
| `death` | The wearer dies |
| `block-break` | The wearer breaks a block |
| `block-place` | The wearer places a block |
| `shoot-bow` | The wearer fires a bow or a crossbow |
| `projectile-hit` | A projectile the wearer fired hits something |
| `interact` | The wearer right clicks |
| `fish` | The wearer catches something with a rod |
| `move` | The wearer moves from one block to another; a turn of the head does not count |

The file format also accepts `periodic`, a timer for an enchantment that acts while worn, but this build never fires it.

Related: [Enchantment files](enchantment-files.md), [types.conf](../config/types-conf.md)
