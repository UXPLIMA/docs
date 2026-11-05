---
title: skygrid.conf
order: 644
description: "SkyGrid: the grid of lone blocks and its chests."
icon: grid-3x3
---

SkyGrid: an island that is a sparse grid of lone blocks hanging in the sky, one every few blocks along each axis. Players arrive on a safe block at its centre and bridge from block to block. A chest in the grid holds a few of the chest items, and a spawner raises one of its dimension's creatures.

The file is `plugins/uxmSkyblock/modules/skygrid.conf`. Every default below is the shipped value.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Whether islands can be made as SkyGrid islands. |
| `spacing` | `4` | How many blocks from one grid block to the next, along each axis. |
| `radius` | `48` | How far the grid reaches sideways from the island's centre. Keep it inside half the grid spacing of the islands, so two islands' grids never meet. |
| `below` | `64` | How far the grid reaches down and up from the block players arrive on. |
| `above` | `32` |  |
| `spawn-block` | `GRASS_BLOCK` | The block players arrive standing on. |

## `chests`

What a chest in the grid holds when its palette names no loot table: rolls of the items, each written ITEM:min:max.

| Key | Default | What it does |
|---|---|---|
| `rolls` | `3` |  |
| `items` | a list of 13 |  |

## `palettes`

The grid of each dimension, by dimension id. A dimension not named here uses the overworld's grid. Each block is written BLOCK:weight: a block of weight 20 stands in the grid twice as often as one of weight 10. spawners lists the creatures a spawner in the grid can raise. biome is the biome the grid stands in; empty leaves the world's own. loot names the loot table under loot/ a chest of the grid is filled from; empty fills it from the chest items above. structures lists small structures a block of the grid may be instead, each written template:chance, the chance from 0 to 1 that one block of the grid is it. A template is one of the server's own or a datapack's, or schematic:`<name>` for schematics/`<name>`.schem: the plugin writes the four shipped ones on its first start. A structure stands on the block's place, centred on it, and is laid only where it fits between that block and the next on every side, inside one chunk; elsewhere the block stays a block. Its chests are filled from loot and its spawners raise the palette's creatures.

## `palettes.overworld`

| Key | Default | What it does |
|---|---|---|
| `blocks` | a list of 25 |  |
| `spawners` | `ZOMBIE`, `SKELETON`, `SPIDER`, `CREEPER` |  |
| `biome` | `plains` |  |
| `loot` | `skygrid` |  |
| `structures` | `schematic:grid_tree:0.006`, `schematic:grid_well:0.003` |  |
| `palettes.the_nether.blocks` | a list of 14 |  |
| `palettes.the_nether.spawners` | `BLAZE`, `MAGMA_CUBE`, `WITHER_SKELETON` |  |
| `palettes.the_nether.biome` | `nether_wastes` |  |
| `palettes.the_nether.loot` | `skygrid_nether` |  |
| `palettes.the_nether.structures` | `schematic:grid_fortress:0.004` |  |
| `palettes.the_end.blocks` | `END_STONE:70`, `END_STONE_BRICKS:6`, `PURPUR_BLOCK:8`, `OBSIDIAN:6`, `CHEST:2` |  |
| `palettes.the_end.spawners` | `ENDERMAN` |  |
| `palettes.the_end.biome` | `the_end` |  |
| `palettes.the_end.loot` | `skygrid_end` |  |
| `palettes.the_end.structures` | `schematic:grid_pillar:0.004` |  |
