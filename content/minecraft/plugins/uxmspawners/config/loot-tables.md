---
title: Loot Tables
order: 158
description: Replacing what a spawner mob drops, the line format, XP, and which mobs a table applies to.
icon: list
---

A loot table replaces what one kind of mob drops when it comes from a uxmSpawners spawner. Files live
in `loottables/`, one per mob, named after the mob: `zombie.yml`, `blaze.yml`, `iron_golem.yml`.

```yaml
drops:
  item-drops:
    - ROTTEN_FLESH:70:1-3
    - GOLD_INGOT:10:1-1
  xp-drops: 1-15
  xp-drop-chance: 100
```

## Item lines

`ITEM:CHANCE:MIN-MAX`

| Part | Meaning |
|---|---|
| `ITEM` | A material name, such as `IRON_INGOT` |
| `CHANCE` | A whole number from 0 to 100: the percent chance this line drops |
| `MIN-MAX` | How many, picked at random between the two. `MAX` must not be below `MIN` |

Each line is rolled on its own, so one death can give every line or none. A line the plugin cannot
read, such as a misspelt material or a chance written as `12.5`, is skipped with a warning in the
console that names the file and the line.

## XP

| Key | Meaning |
|---|---|
| `xp-drops` | A range such as `1-15` |
| `xp-drop-chance` | The percent chance of giving it. Without this key, the mob gives no XP |

The 4.2.0 names `itemDrops`, `xpDrops` and `xpDropChance` are still read.

## Which mobs it applies to

| Mob | Drops |
|---|---|
| From a uxmSpawners spawner, killed by Auto Kill | The table |
| From a uxmSpawners spawner, killed by a player | Its normal drops, or the table with `loot-table-only-auto-kill: false` |
| From a natural spawner, or spawned naturally | Its normal drops. Tables never apply |

`loot-table-only-auto-kill` is in the `auto-kill` section of `general-settings.yml`.

Without a table for a mob, a spawner mob drops its normal drops, and with modern Auto Kill the game's
own loot table for that mob is rolled.

The amounts and XP are multiplied by the spawner's level, except items in
`level-scaling.never-multiplied-items`, and with modern Auto Kill also by its Spawn Amount boost.

## Making one

`loottables/example_zombie.yml` ships as a working reference. Its name is not a mob, so it is never
used by itself: copy it to `zombie.yml`, edit it, and run `/usp reload`.

<Callout type="tip" title="Tables set the economy of farms">

A table is where a farm's income is decided: what drops, how often, and how much it sells for at
your prices. Work out what one spawner earns in an hour at its `delay` before raising a chance.

</Callout>

Related: [Auto Kill](../features/auto-kill.md), [Prices](prices.md)
