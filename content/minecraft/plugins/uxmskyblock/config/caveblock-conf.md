---
title: caveblock.conf
order: 618
description: "CaveBlock: the rock, the room and its tunnels."
icon: mountain
---

CaveBlock: an island enclosed in rock. Players arrive in a room carved out of it, with tunnels winding away and ravines cut deep through it, and mine outward through rock and ores. The rock is closed in by a shell below, above and on every side.

The file is `plugins/uxmSkyblock/modules/caveblock.conf`. Every default below is the shipped value.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Whether islands can be made as CaveBlock islands. |
| `shell` | `BEDROCK` | The block the rock is closed in by. |

## `size`

How far the rock reaches from the block players arrive standing on. Keep the radius inside half the grid spacing, so two islands' rock never meets.

| Key | Default | What it does |
|---|---|---|
| `radius` | `32` |  |
| `below` | `24` |  |
| `above` | `16` |  |

## `room`

The room players arrive in.

| Key | Default | What it does |
|---|---|---|
| `radius` | `4` |  |

## `tunnels`

Tunnels that wind out of the room.

| Key | Default | What it does |
|---|---|---|
| `count` | `6` |  |
| `length` | `40` |  |

## `ravines`

Ravines cut deep through the rock.

| Key | Default | What it does |
|---|---|---|
| `count` | `2` |  |
| `length` | `30` |  |
| `depth` | `14` |  |

## `palettes`

The rock of each dimension, by dimension id. A dimension not named here uses the overworld's rock. Each ore is written BLOCK:chance, the chance that one block of the rock is that ore, or BLOCK:chance:below for an ore found only from below blocks under the arrival down. The chances of one rock add up to at most 1. deep-rock and deep-ores take over deep-below blocks under the arrival. richer makes the ores richer further down: their chances rise evenly from the arrival, where they are as written, to 1 + richer times as written at the floor of the rock. 0 keeps them as written.

## `palettes.overworld`

| Key | Default | What it does |
|---|---|---|
| `rock` | `STONE` |  |
| `ores` | a list of 7 |  |
| `deep-rock` | `DEEPSLATE` |  |
| `deep-below` | `12` |  |
| `deep-ores` | a list of 5 |  |
| `richer` | `1` |  |
| `palettes.the_nether.rock` | `NETHERRACK` |  |
| `palettes.the_nether.ores` | `NETHER_QUARTZ_ORE:0.016`, `NETHER_GOLD_ORE:0.008`, `ANCIENT_DEBRIS:0.0004:12` |  |
| `palettes.the_nether.richer` | `1` |  |
| `palettes.the_end.rock` | `END_STONE` |  |
| `palettes.the_end.ores` | empty list |  |

## `structures`

Structures the rock holds, laid after it around the island: count of each kind, spread evenly at distance from the centre, its floor depth blocks under where players arrive. A template is one of the server's own or a datapack's, or schematic:`<name>` for the file schematics/`<name>`.schem under the plugin's folder: the plugin writes the four shipped ones on its first start, and you may edit them with any tool that writes a Sponge schematic, or with /is schematic save. Inside a schematic, cave air is the hollow it carves out of the rock; air leaves the rock as it stands. Each chest and barrel gets a loot table from loot, in turn, and each spawner and trial spawner the creature spawner names. A kind that would not fit inside the shell is not laid. A shipped kind left out of this file stays as shipped; give it count = 0 to stop it.

## `structures.dungeon`

| Key | Default | What it does |
|---|---|---|
| `templates` | `schematic:cave_dungeon` |  |
| `count` | `1` |  |
| `distance` | `16` |  |
| `depth` | `10` |  |
| `loot` | `minecraft:chests/simple_dungeon` |  |
| `spawner` | `zombie` |  |
| `structures.mineshaft.templates` | `schematic:cave_mineshaft` |  |
| `structures.mineshaft.count` | `2` |  |
| `structures.mineshaft.distance` | `18` |  |
| `structures.mineshaft.depth` | `3` |  |
| `structures.mineshaft.loot` | `minecraft:chests/abandoned_mineshaft` |  |
| `structures.mineshaft.spawner` | empty |  |
| `structures.geode.templates` | `schematic:cave_geode` |  |
| `structures.geode.count` | `1` |  |
| `structures.geode.distance` | `20` |  |
| `structures.geode.depth` | `16` |  |
| `structures.geode.loot` | empty list |  |
| `structures.geode.spawner` | empty |  |
| `structures.trial_chamber.templates` | `schematic:cave_trial_chamber` |  |
| `structures.trial_chamber.count` | `1` |  |
| `structures.trial_chamber.distance` | `18` |  |
| `structures.trial_chamber.depth` | `20` |  |
| `structures.trial_chamber.loot` | `minecraft:chests/trial_chambers/reward` |  |
| `structures.trial_chamber.spawner` | `breeze` |  |

## `zones`

Zones of the cave, each with its own look. Each zone takes an even share of the round of the island, outside the room players arrive in, in the order of the zones' names and turned by the island. Its blocks are the biome it names (empty leaves the island's own); deep = true lays the whole zone as the dimension's deep rock with its deep ores. A floor of rock is covered, by chance, in a floor block and has an on-floor block stood on it; a ceiling is covered in a ceiling block with an under-ceiling block hanging below it. Each is written BLOCK:chance. A zone is in the dimensions it lists. A shipped zone left out of this file stays as shipped; give it dimensions = [] to stop it.

## `zones.lush`

| Key | Default | What it does |
|---|---|---|
| `biome` | `lush_caves` |  |
| `deep` | `false` |  |
| `floor` | `MOSS_BLOCK:0.8`, `CLAY:0.08` |  |
| `on-floor` | `MOSS_CARPET:0.25`, `AZALEA:0.04`, `FLOWERING_AZALEA:0.03`, `SHORT_GRASS:0.1` |  |
| `ceiling` | `MOSS_BLOCK:0.5` |  |
| `under-ceiling` | `CAVE_VINES:0.12`, `SPORE_BLOSSOM:0.01` |  |
| `dimensions` | `overworld` |  |
| `zones.dripstone.biome` | `dripstone_caves` |  |
| `zones.dripstone.deep` | `false` |  |
| `zones.dripstone.floor` | `DRIPSTONE_BLOCK:0.6` |  |
| `zones.dripstone.on-floor` | `POINTED_DRIPSTONE:0.15` |  |
| `zones.dripstone.ceiling` | `DRIPSTONE_BLOCK:0.6` |  |
| `zones.dripstone.under-ceiling` | `POINTED_DRIPSTONE:0.2` |  |
| `zones.dripstone.dimensions` | `overworld` |  |
| `zones.deep_dark.biome` | `deep_dark` |  |
| `zones.deep_dark.deep` | `true` |  |
| `zones.deep_dark.floor` | `SCULK:0.7` |  |
| `zones.deep_dark.on-floor` | `SCULK_SENSOR:0.02`, `SCULK_SHRIEKER:0.005` |  |
| `zones.deep_dark.ceiling` | `SCULK:0.3` |  |
| `zones.deep_dark.under-ceiling` | empty list |  |
| `zones.deep_dark.dimensions` | `overworld` |  |

## `pools`

Pools of liquid in the rock, each a flattened sphere radius blocks wide and half as high, its middle between depth-from and depth-to blocks under where players arrive, well away from the room and the shell. fill is how much of its height, from the bottom, is the liquid, from 0 to 1, with air above: a lake is half full, a lava pocket full to the top and found by mining into it. A pool that cannot fit in the rock is not laid. A pool is in the dimensions it lists. A shipped pool left out of this file stays as shipped; give it count = 0 to stop it.

## `pools.lake`

| Key | Default | What it does |
|---|---|---|
| `block` | `WATER` |  |
| `count` | `3` |  |
| `radius` | `4` |  |
| `fill` | `0.5` |  |
| `depth-from` | `2` |  |
| `depth-to` | `16` |  |
| `dimensions` | `overworld` |  |
| `pools.lava.block` | `LAVA` |  |
| `pools.lava.count` | `2` |  |
| `pools.lava.radius` | `2` |  |
| `pools.lava.fill` | `1` |  |
| `pools.lava.depth-from` | `10` |  |
| `pools.lava.depth-to` | `20` |  |
| `pools.lava.dimensions` | `overworld`, `the_nether` |  |
