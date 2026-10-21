---
title: Fly, Effects & Stacking
order: 730
description: Flight on a player's own island, the potion effects an island's upgrades give, and
  blocks that stack into one.
icon: feather
---

## Flight

`/is fly` flies a player where they stand on an island they belong to, or brings them down. It is
set in `modules/fly.conf`. A player flies when all of these hold:

- their island role holds the fly permission, which the owner always does;
- they hold the node `fly.permission`, `uxmskyblock.island.fly` as shipped, or it is empty;
- the island holds a tier of `fly.required-upgrade`, or it is empty.

Leaving that ground sets them down, and `fly.spare-fall` spares their fall. Flight a player has
from elsewhere, from creative or another plugin, is left alone.

## Island effects

`modules/island-effects.conf` turns upgrades into potion effects on the players standing on the
island. Every upgrade named in `island-effects.upgrades` is an effect upgrade: each property of its
tiers in `modules/upgrades.conf` is a potion effect and its level, so `haste = 2` is Haste II.

```hocon
island_effects {
    tiers = [
        { cost = 50000, currency = "PRIMARY", properties { haste = 1 } },
        { cost = 150000, currency = "PRIMARY", properties { haste = 1, speed = 1 } }
    ]
}
```

| Setting | Shipped | What it does |
| --- | --- | --- |
| `every-seconds` | `3` | How often the effects are put on again. They wear off about this long after a player leaves |
| `visitors` | `false` | Whether visitors wear them too, or only the island's members |
| `upgrades` | `["island_effects"]` | The upgrades whose tiers name effects |

## Block stacking

A block of a kind `modules/stacking.conf` lists, placed on a block of the same kind, joins it: the
block stays one block in the world and stands for one more. Each block in a stack counts toward
the island's worth and level, a line above it shows how many it stands for, and breaking it gives
back one at a time. A player who sneaks places a block the usual way.

| Setting | Shipped | What it does |
| --- | --- | --- |
| `stacking.blocks` | the ten storage blocks, iron to amethyst | The blocks that stack |
| `stacking.most` | `100000` | The most blocks one stack stands for |
