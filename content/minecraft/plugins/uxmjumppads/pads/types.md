---
title: Pad types
order: 401
description: The two shipped types, every key of a type, how far a speed carries, and a type for some players only.
icon: shapes
---

A pad type is one block under `pads` in [config.conf](../config/config-conf.md#pads). The key on the left is its
name, which `set` uses. Every block of the server that is the type's `block` on its `under` is a pad of that type.

## The shipped types

| Type | Block | Under | Forward | Upward | Effects |
|---|---|---|---|---|---|
| `classic` | `LIGHT_WEIGHTED_PRESSURE_PLATE` | `REDSTONE_BLOCK` | 1.6 | 0.9 | Wind burst sound, 20 cloud particles |
| `bounce` | `SLIME_BLOCK` | `EMERALD_BLOCK` | 0 | 1.4 | Slime fall sound, 15 slime particles |

## The keys

| Key | Left out | What it is |
|---|---|---|
| the block name | | The type's name: lower-case letters, digits, `-` and `_` |
| `block` | required | The block that is the pad. A plate, carpet or button is walked into; a full block is stood on |
| `under` | `""`, any | The block that must sit under the pad. Keep it, or every block of the kind on the server throws |
| `forward` | `1.5` | Speed along the throw, in blocks per tick, `0` to `4`. `0` throws straight up |
| `upward` | `1.0` | Speed upward, in blocks per tick, `0` to `4` |
| `permission` | `""`, everybody | The node a player needs to be thrown |
| `effects` | `[]` | Action lines run as the pad throws. See [Effects](effects.md) |

A block name is a material, with or without `minecraft:`, in any case. A type whose `block` or `under` is not a
block, whose speeds are outside `0` to `4`, or whose speeds are both `0`, is left out: the console and
`/uxmjumppads doctor` name it and why, and the other types keep working.

When two types match the same blocks, the first in the file wins.

## How far a speed carries

`upward` is the launch speed, not a height. As a rough guide, from the game's own gravity and drag:

| `upward` | About this high |
|---|---|
| 0.5 | 2 blocks |
| 0.9 | 5 blocks |
| 1.4 | 10 blocks |
| 2.0 | 20 blocks |

`forward` carries further the longer the player is in the air, so a higher `upward` also lands further away. Try a
value, walk onto the pad, and adjust: `/uxmjumppads reload` applies it at once.

## A type for some players

```hocon
vip {
  block      = "HEAVY_WEIGHTED_PRESSURE_PLATE"
  under      = "DIAMOND_BLOCK"
  forward    = 2.0
  upward     = 1.0
  permission = "lobby.pads.vip"
  effects    = ["[sound] entity.firework_rocket.launch 1 1"]
}
```

A player without the node walks over the pad as over any plate, and is told nothing. The node is yours to name and to
grant; uxmJumpPads does not declare it.

## The map is yours

The `pads` map is never merged: a type you delete stays deleted, and the next start does not put `classic` or
`bounce` back.

Related: [Set pads](set-pads.md), [Throws and landings](throws.md), [config.conf](../config/config-conf.md#pads)
