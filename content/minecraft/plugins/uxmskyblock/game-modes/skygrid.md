---
title: SkyGrid
order: 60
description: An island that is a sparse grid of lone blocks hanging in the sky, bridged block by
  block, with chests, spawners and small structures.
icon: grid-2x2
---

A SkyGrid island is a sparse grid of lone blocks hanging in the sky, one every few blocks along
each axis. Players arrive on a safe block at its centre and bridge from block to block. A chest in
the grid holds a few of the chest items, and a spawner raises one of its dimension's creatures.

## Turning it on

1. Set `enabled = true` in `modules/skygrid.conf`.
2. Add `skygrid` to `presets.modes` in `modules/presets.conf`.

The shipped preset `skygrid` starts with `"uxm:skygrid"`. The Nether and the End are grids of
their own blocks.

## The grid

| Setting | Shipped | What it does |
| --- | --- | --- |
| `spacing` | `4` | How many blocks from one grid block to the next, along each axis |
| `radius` | `48` | How far the grid reaches sideways. Keep it inside half the grid spacing of the islands |
| `below`, `above` | `64`, `32` | How far it reaches down and up from the arrival |
| `spawn-block` | `GRASS_BLOCK` | The block players arrive standing on |

## Chests

A chest of the grid whose palette names no loot table holds `chests.rolls` rolls of
`chests.items`, each written `ITEM:min:max`, as `"IRON_INGOT:1:3"`.

## Palettes

`palettes` holds the grid of each dimension by its id. A dimension not named uses the overworld's.

```hocon
overworld {
    blocks = [ "DIRT:40", "STONE:40", "OAK_LOG:15", "DIAMOND_ORE:1", "CHEST:2", "SPAWNER:1" ]
    spawners = [ "ZOMBIE", "SKELETON", "SPIDER", "CREEPER" ]
    biome = "plains"
    loot = "skygrid"
    structures = [ "schematic:grid_tree:0.006", "schematic:grid_well:0.003" ]
}
```

| Key | What it does |
| --- | --- |
| `blocks` | Each `BLOCK:weight`. A block of weight 20 stands in the grid twice as often as one of 10 |
| `spawners` | The creatures a spawner of the grid can raise |
| `biome` | The biome the grid stands in. Empty leaves the world's own |
| `loot` | The loot table under `loot/` a chest is filled from. Empty fills it from the chest items |
| `structures` | Small structures a block may be instead, each `template:chance` |

A structure's template is one of the server's own, a data pack's, or `schematic:<name>` for
`schematics/<name>.schem`. The plugin writes the four shipped ones on its first start. A structure
stands on the block's place, centred on it, and is laid only where it fits between that block and
the next on every side, inside one chunk. Elsewhere the block stays a block. Its chests are filled
from `loot` and its spawners raise the palette's creatures.
