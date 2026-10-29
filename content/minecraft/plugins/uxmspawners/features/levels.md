---
title: Levels
order: 33
description: Stacking identical spawners into one, what a level multiplies, and breaking levels off again.
icon: layers
---

Instead of placing ten zombie spawners side by side, a player stacks them into one at level 10. It
produces what the ten would, from one block, into one storage, with one menu to empty.

## Levelling up

The owner opens the spawner and presses **Level Up**. Every spawner item in their inventory that is
identical to the placed one is merged into it, one level each, until the limit.

| The owner is told | Why |
|---|---|
| How many levels were added | It worked |
| No matching spawners were found | None in the inventory are identical to this one |
| The spawner is at the maximum level | `level-limit` is reached |
| They cannot do this with a spawner in the off hand | Move spawners out of the off hand and the armour slots first |
| They are not the owner | Friends cannot level up someone else's spawner |

Identical means the same item: the same type, name and lore. A spawner item handed out before you
changed that type's name in the config no longer matches one handed out after.

```yaml
spawner-level-limit:
  level-limit: 256
```

in `modules/other.yml`.

## What a level multiplies

| Spawner | Effect of the level |
|---|---|
| `ENTITY` with auto-kill | The loot and XP of every cycle |
| `ENTITY` without auto-kill | The drops and XP of each mob when it is killed. The number of mobs does not change |
| `ITEM` | The number of items each cycle |

The level does not raise the storage limit. A level 50 spawner fills its storage fifty times faster,
so storage tiers matter more on servers with high levels.

## Breaking levels off

| How | Result |
|---|---|
| Break the spawner | One level comes off, and one spawner item drops |
| Sneak and break | Every level above 1 comes off and drops, and the spawner stays at level 1 |
| Break a level 1 spawner | The block is removed, with its storage, and one spawner item drops |

Each item that would drop is rolled against the player's drop chance, so with a chance below 100 some
of the levels can be lost. See [Breaking Rules](breaking.md#drop-chance).

Related: [Basic Concepts](../getting-started/concepts.md), [Storage & Selling](storage.md)
