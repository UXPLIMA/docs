---
title: dimensions.conf
order: 621
description: "The Nether and the End of an island: own, shared or none."
icon: orbit
---

The Nether and the End of an island: whether each island has its own, everyone shares one, or there is none.

The file is `plugins/uxmSkyblock/modules/dimensions.conf`. Every default below is the shipped value.

## `dimensions`

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | false keeps every island to the overworld. |

## `dimensions.nether`

Nether dimension configuration

| Key | Default | What it does |
|---|---|---|
| `mode` | `PRIVATE_ISLAND` | Mode: PRIVATE_ISLAND (1-to-1 coordinate mirror), SHARED_WORLD (centralized wilderness), or DISABLED |
| `world-name` | `skyblock_nether` | Destination world name |
| `required-upgrade` | `island_nether` | Upgrade required in upgrades.conf to unlock portal access (or empty for unrestricted) |
| `schematic` | `island_nether` | The schematic an island's platform here is pasted from, by its name in schematics/: island_nether is schematics/island_nether.schem. The plugin writes the platform it ships there on its first start. Without the file the shipped platform is laid. |

## `dimensions.end`

The End dimension configuration

| Key | Default | What it does |
|---|---|---|
| `mode` | `PRIVATE_ISLAND` | Mode: PRIVATE_ISLAND (1-to-1 coordinate mirror), SHARED_WORLD (centralized wilderness), or DISABLED |
| `world-name` | `skyblock_the_end` | Destination world name |
| `required-upgrade` | `island_end` | Upgrade required in upgrades.conf to unlock portal access (or empty for unrestricted) |
| `schematic` | `island_end` | The schematic an island's platform here is pasted from, as above: schematics/island_end.schem. |
