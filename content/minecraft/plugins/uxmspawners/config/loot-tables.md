---
title: Loot Tables
order: 58
description: Writing a loot table, and what the chance and range fields mean.
icon: list
---

A loot table replaces what a mob drops into spawner storage. Files live in `loottables/`, one per
mob, named after the entity type: `zombie.yml`, `blaze.yml`.

```yaml
drops:
  itemDrops:
    - ROTTEN_FLESH:70:1-3
    - GOLD_INGOT:10:1-1
  xpDrops: 1-15
  xpDropChance: 100
```

## The drop format

`ITEM:CHANCE:RANGE`

| Part | Meaning |
|---|---|
| `ITEM` | A material name |
| `CHANCE` | 0–100, the percent chance this line drops at all |
| `RANGE` | `min-max` quantity, e.g. `1-3` |

Each line is rolled on its own, so a mob can drop everything or nothing.

## XP

`xpDrops` is a range and `xpDropChance` is the percent chance of rolling it.

## Scope

`modules/autokill.yml → loottable-only-autokill` decides who these apply to:

| Value | Effect |
|---|---|
| `true` *(default)* | Loot tables apply to auto-killed mobs. A player who kills a mob by hand gets vanilla drops |
| `false` | Loot tables apply to both |

Leaving it `true` is what lets you tune farm output hard without changing what mob drops mean
everywhere else on the server.

<Callout type="tip" title="Start from the example">

`loottables/example_zombie.yml` ships as a working reference. Copy it to `zombie.yml`: the
example file name does not match an entity, so it is never used by itself.

</Callout>
