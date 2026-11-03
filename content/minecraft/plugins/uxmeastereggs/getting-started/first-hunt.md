---
title: Your first hunt
order: 104
description: Place a few eggs, check them, and set the rewards of the hunt.
icon: flag
---

This walk-through hides three eggs around the spawn with the shipped types.

## 1. Put the blocks down

Place the blocks that will be eggs. A player head reads best; give it a texture with any head plugin, or use the
texture of your type file, which the menus draw. Hide them where a player has to look: behind a sign, in a tree, on a
roof.

## 2. Register each one

Look at the block, within six blocks, and type:

```
/uxmeastereggs create common
```

The reply names the type, the position and the egg's id: *common is placed at 12 70 -4, as 3f2a91c0.* Type
`/uxmeastereggs create` with no type to choose one from the [type picker](../menus/picker.md) instead. From the
console, name the block: `/uxmeastereggs create golden 12 70 -4 world`.

## 3. Check them

| Command | Shows |
|---|---|
| `/uxmeastereggs list` | Every egg with its id, type, world and position |
| `/uxmeastereggs show 3f2a91c0` | One egg |
| `/uxmeastereggs editor` | The [placed eggs](../menus/placed-eggs.md) window: click one to go there |
| `/uxmeastereggs tp 3f2a91c0` | Stands you on top of it |

## 4. Find one

Click an egg as a player who is not an operator. You hear the pickup sound, read *You found a Common Egg* and
*You have found 1 of 3 eggs*, and earn 50 experience points. `/eggs` shows your collection.

## 5. Set the rewards

| Where | Shipped |
|---|---|
| `types/common.conf`, `reward` | 50 experience points per find |
| `types/golden.conf`, `reward` | 500 experience points per find |
| `config.conf`, `milestones` | 250 experience at 5 eggs, 1000 at 25 with a broadcast |
| `config.conf`, `completion` | 5000 experience for every egg, with a broadcast |

Change them and run `/uxmeastereggs reload`. See [Rewards](../hunting/rewards.md).

## Next

- Write a type of your own: [Type files](../config/type-files.md).
- Let players hunt with a hint: [Radar and particles](../hunting/radar-and-particles.md).
- Put the top finders on a hologram: [Placeholders](../placeholders/reference.md).
