---
title: Levels
order: 133
description: Stacking identical spawners into one, what a level multiplies, and breaking levels off again.
icon: layers
---

Instead of placing ten zombie spawners side by side, a player stacks them into one at level 10. It
produces what the ten would, from one block, into one storage, with one menu to empty.

## Levelling up

The owner opens the spawner and presses **Level Up**. Every spawner item in their inventory and off
hand that is identical to the placed one is merged into it, one level each, until the limit. Spawners
worn as a hat are left alone.

| The owner is told | Why |
|---|---|
| How many levels were added | It worked |
| No matching spawners were found | None in the inventory are identical to this one |
| The spawner is at its maximum level | `level-limit` is reached |
| Only the owner can do that | Friends cannot level up someone else's spawner |

Identical means the same item: the same type, name and lore. A spawner item handed out before you
changed that type's name in its file no longer matches one handed out after.

```yaml
spawner-level-limit:
  level-limit: 256
```

in `general-settings.yml`. Placing a spawner on top of another does not stack it: only **Level Up**
does.

## What a level multiplies

| Spawner | Effect of the level |
|---|---|
| `ENTITY` with modern Auto Kill | The loot and XP of every cycle |
| `ENTITY`, other | The drops and XP of each mob, or the number of mobs with `level-scaling.mode: MOB_COUNT` |
| `ITEM` | The number of items each cycle |

Drops in `never-multiplied-items`, such as mob heads, come once whatever the level. See
[Production](production.md#what-a-level-multiplies).

The level does not raise the storage limit unless `storage-limit.multiply-by-level` is on. Without
it, a level 50 spawner fills its storage fifty times faster. See [Storage & Selling](storage.md#limits).

## Breaking levels off

| How | Result |
|---|---|
| Break the spawner | One level comes off as one spawner item |
| Sneak and break | Every level above 1 comes off, and the spawner stays at level 1 |
| Break a level 1 spawner | The block is removed and one spawner item drops. Its storage is handled as `break-with-storage` says |

Each item that would drop is rolled against the player's drop chance, so with a chance below 100 some
of the levels can be lost. See [Breaking Rules](breaking.md#drop-chance).

Related: [Basic Concepts](../getting-started/concepts.md), [Storage & Selling](storage.md)
