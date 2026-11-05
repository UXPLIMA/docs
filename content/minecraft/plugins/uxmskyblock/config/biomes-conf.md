---
title: biomes.conf
order: 614
description: Which biomes an island may take, and what it must reach first.
icon: trees
---

Island biomes: which biomes an island may take, and what it has to reach first.

required-level is the island level the biome asks for. Zero means anyone. offered = false takes the biome off this server entirely, whatever the level.

A node with no level engine has no levels to require, so every offered biome is open there. The biome menu is menus/island-biome.conf and it is yours to rewrite: taking a biome off here does not remove its button, it refuses it.

The file is `plugins/uxmSkyblock/modules/biomes.conf`. Every default below is the shipped value.

## `biomes`

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Whether islands may change their biome on this server at all. |
| `unlocks.plains.required-level` | `0` |  |
| `unlocks.plains.offered` | `true` |  |
| `unlocks.desert.required-level` | `5` |  |
| `unlocks.desert.offered` | `true` |  |
| `unlocks.nether_wastes.required-level` | `10` |  |
| `unlocks.nether_wastes.offered` | `true` |  |
| `unlocks.jungle.required-level` | `15` |  |
| `unlocks.jungle.offered` | `true` |  |
| `unlocks.snowy_plains.required-level` | `20` |  |
| `unlocks.snowy_plains.offered` | `true` |  |
| `unlocks.flower_forest.required-level` | `25` |  |
| `unlocks.flower_forest.offered` | `true` |  |
| `unlocks.swamp.required-level` | `30` |  |
| `unlocks.swamp.offered` | `true` |  |
| `unlocks.the_end.required-level` | `50` |  |
| `unlocks.the_end.offered` | `true` |  |
