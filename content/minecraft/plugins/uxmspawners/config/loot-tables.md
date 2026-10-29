---
title: Loot Tables
order: 58
description: Replacing what a spawner mob drops, the line format, XP, and which mobs a table applies to.
icon: list
---

A loot table replaces what one kind of mob drops when it comes from a uxmSpawners spawner. Files live
in `loottables/`, one per mob, named after the mob: `zombie.yml`, `blaze.yml`, `iron_golem.yml`.

```yaml
drops:
  itemDrops:
    - ROTTEN_FLESH:70:1-3
    - GOLD_INGOT:10:1-1
  xpDrops: 1-15
  xpDropChance: 100
```

## Item lines

`ITEM:CHANCE:MIN-MAX`

| Part | Meaning |
|---|---|
| `ITEM` | A material name, such as `IRON_INGOT` |
| `CHANCE` | A whole number from 0 to 100: the percent chance this line drops |
| `MIN-MAX` | How many, picked at random between the two. `MAX` must not be below `MIN` |

Each line is rolled on its own, so one death can give every line or none. A line the plugin cannot
read is skipped without a warning: a misspelt material, a chance written as `12.5`, or a missing part.
Check a new table in game.

## XP

| Key | Meaning |
|---|---|
| `xpDrops` | A range such as `1-15` |
| `xpDropChance` | The percent chance of giving it. Without this key, the mob gives no XP |

## Which mobs it applies to

| Mob | Drops |
|---|---|
| From a uxmSpawners spawner, auto-killed | The table |
| From a uxmSpawners spawner, killed by a player | The table |
| From a vanilla spawner, or spawned naturally | Its normal drops. Tables never apply |

`loottable-only-autokill` in `modules/autokill.yml` suggests otherwise, but it is not read in 4.2.0.

Without a table for a mob, a spawner mob drops its normal drops, and with modern auto-kill the game's
own loot table for that mob is rolled.

The amounts and XP are multiplied by the spawner's level, and with modern auto-kill also by its
spawn-amount boost.

## Making one

`loottables/example_zombie.yml` ships as a working reference. Its name is not a mob, so it is never
used by itself: copy it to `zombie.yml`, edit it, and run `/uxmspawners reload`.

<Callout type="tip" title="Tables set the economy of farms">

A table is where a farm's income is decided: what drops, how often, and how much it sells for at
your prices. Work out what one spawner earns in an hour at its `delay` before raising a chance.

</Callout>

Related: [Auto-Kill](../features/auto-kill.md), [Prices](prices.md)
