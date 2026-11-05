---
title: acidisland.conf
order: 610
description: "AcidIsland: the acid sea, acid rain, clean water and the vent."
icon: droplets
---

AcidIsland: an island that stands in an acid sea. The sea burns whoever swims in it, and acid rain burns whoever it falls on. Water taken from the sea is acid too: a bottle of it poisons the one who drinks it until it is made clean. Rain a cauldron catches is clean already, but a cauldron a bucket of sea water was poured into holds the sea until it is emptied.

The file is `plugins/uxmSkyblock/modules/acidisland.conf`. Every default below is the shipped value.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Whether islands can be made as AcidIsland islands. |

## `sea`

The sea an AcidIsland island is made in. It fills the air around the island and never replaces a block the island already has.

| Key | Default | What it does |
|---|---|---|
| `below-island` | `2` | How many blocks below the island's start height the surface stands. |
| `depth` | `4` | How many blocks of water, above the floor. |
| `radius` | `64` | How far from the island's centre the sea reaches. Keep it inside half the grid spacing, so two islands' seas never meet. |
| `floor` | `SANDSTONE` | The block the sea stands on. A block that falls, such as sand or gravel, would drop out from under the sea and let it drain away, so one of those is replaced by sandstone. |

## `sea.vents`

Sulfur vents: magma on the floor that pulls a swimmer down, and fumes that reach whoever is near.

| Key | Default | What it does |
|---|---|---|
| `chance` | `0.004` | The chance that one column of the floor is a vent. 0 lays none. |
| `reach` | `3` | How many blocks sideways the fumes of a vent reach. |
| `effects` | `nausea:0:6`, `poison:1:3` | Effects the fumes give, each written name:amplifier:seconds. No effect keeps them off. |

## `sea.vents.offerings`

Offerings: an item thrown into the sea that sinks onto a vent and lies there as long as after is taken by it, if an offering below takes it, and a roll of that offering's loot table comes back to whoever threw it, while they are near. Only a player's own throw is an offering: what a creature drops or a dispenser throws, and what another player threw, dissolves as any item. The first offering in the list whose items name the item answers it, and takes as many of them as takes, from 1 to 64; fewer than that are not enough. An item no offering takes dissolves as any other. list = [] takes nothing.

| Key | Default | What it does |
|---|---|---|
| `after` | `2s` |  |
| `list` | a list of 3 entries, below |  |

`sea.vents.offerings.list`:

| # | `items` | `takes` | `loot` |
|---|---|---|---|
| 1 | `["minecraft:bone", "minecraft:rotten_flesh", "minecraft:spider_eye"]` | `8` | `minecraft:chests/underwater_ruin_small` |
| 2 | `["minecraft:gold_ingot"]` | `1` | `minecraft:chests/buried_treasure` |
| 3 | `["minecraft:diamond", "minecraft:emerald"]` | `1` | `minecraft:chests/underwater_ruin_big` |

## `sea.geysers`

Geysers: soul sand on the floor that throws a swimmer up out of the sea.

| Key | Default | What it does |
|---|---|---|
| `chance` | `0.004` | The chance that one column of the floor is a geyser. 0 lays none. |

## `sea.bed`

The sea's floor. blocks are what it is made of, each written BLOCK weight: each column takes one by its weight, the same for the same island every time. A block that falls, as sand, is left out, and with none left the floor is the floor above. thickness is how many blocks thick it is, from 1 to 16. mounds is how far it rises at most, in gentle mounds, from 0 to 15; a block of water always stays above, and vents and geysers lie only where it does not rise.

| Key | Default | What it does |
|---|---|---|
| `blocks` | `SANDSTONE 6`, `SMOOTH_SANDSTONE 2`, `TUFF 2`, `CLAY 1`, `YELLOW_TERRACOTTA 1` |  |
| `thickness` | `3` |  |
| `mounds` | `2` |  |

## `sea.buried`

Structures buried in the floor around the island, laid as Poseidon lays its wrecks: each group lays count of its templates, by key, distance blocks from the centre and spread evenly around it, and gives each chest a loot table from loot, in turn. sink is how deep each sits in the floor, from 0 to 16; a structure fills the floor's blocks and water, keeps the island, and reaches no lower than the floor's bottom. Keep distance and the template inside the radius. groups = `()` buries nothing.

| Key | Default | What it does |
|---|---|---|
| `sink` | `2` |  |
| `groups.ruins.templates` | a list of 8 |  |
| `groups.ruins.count` | `4` |  |
| `groups.ruins.distance` | `34` |  |
| `groups.ruins.loot` | `minecraft:chests/underwater_ruin_small` |  |
| `groups.temple.templates` | a list of 4 |  |
| `groups.temple.count` | `1` |  |
| `groups.temple.distance` | `48` |  |
| `groups.temple.loot` | `minecraft:chests/underwater_ruin_big` |  |

## `sea.tide`

The acid tide. The sea rises rise blocks over half of period and falls back over the other half, a block at a time, by the clock, so every server of a network keeps the same tide. Water is added only over the sea, never on an island, and taken away again as the tide falls. It is brought to the tide's level every check-every, around the islands players are on. rise = 0 keeps the sea still. The effects acid-tide-rising and acid-tide-falling fire at each block.

| Key | Default | What it does |
|---|---|---|
| `rise` | `2` |  |
| `period` | `20m` |  |
| `check-every` | `10s` |  |

## `sea.between`

A sea between islands. With enabled, every chunk of world the server makes is laid with the sea, its floor, vents and geysers as above, at surface, so the islands of that world stand in one sea that runs on as far as players go, and a boat goes from island to island. All of that world's water is acid then, on an island or between them, and rises with the tide. Give the AcidIsland preset that world in presets.conf, keep other modes out of it, and set surface to the islands' sea: their start height less below-island, 98 for the shipped height. Chunks the world made before this was on are left as they are, so start it on a new world.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `false` |  |
| `world` | empty |  |
| `surface` | `98` |  |

## `sea.look`

How the sea looks. biome is painted over the sea, from its floor to the sky, when an island is made. "uxmskyblock:sulfur_sea" is the plugin's own, drawn in the colours below, each written #rrggbb: water, water-fog seen from under it, fog above it and sky. The plugin writes it into a data pack at start, so a change to the colours shows after a restart. Any biome the game or a data pack has may be named instead, and "" leaves the biome the world made. A sea painted with the plugin's biome keeps it in its chunks; with the plugin gone, the game reads it as plains.

| Key | Default | What it does |
|---|---|---|
| `biome` | `uxmskyblock:sulfur_sea` |  |
| `water` | `#ffe66d` |  |
| `water-fog` | `#565f6b` |  |
| `fog` | `#fff3a8` |  |
| `sky` | `#e8ecf5` |  |
| `hazard.check-every` | `1s` | How often each player on an AcidIsland island is checked. |
| `hazard.water-damage` | `2.0` | Health taken at each check from a player in the sea. A heart is 2. |
| `hazard.rain-damage` | `1.0` | Health taken at each check from a player the rain falls on. |
| `hazard.helmet-blocks-rain` | `true` | Whether anything worn on the head keeps the rain off. |
| `hazard.water-protection` | `water_breathing` | Effects, by name, that keep a player safe in the sea. |
| `hazard.water-effects` | `poison:0:3` | Effects the sea gives, each written name:amplifier:seconds. |

## `hazard.armour`

A full set of armour, all four pieces, of one of these sets keeps the acid off. A set is named as each piece's name starts: "netherite" for netherite_helmet and the rest, as "diamond", "iron", "golden", "chainmail", "leather" or "copper". sets = [] lets no armour keep it off.

| Key | Default | What it does |
|---|---|---|
| `sets` | `netherite` |  |
| `sea` | `true` | Whether such a set keeps the sea off. |
| `rain` | `true` | Whether such a set keeps the rain off. |
| `wear` | `1` | The durability each piece loses at each check it keeps the acid off. 0 wears nothing. |

## `sea-life`

What the sea does to what lies and swims in it, looked at within reach blocks of each player on an AcidIsland island at each check. reach = 0 looks at nothing.

| Key | Default | What it does |
|---|---|---|
| `reach` | `32` |  |
| `items.dissolve` | `true` | Whether an item lying in the sea dissolves, once it has lain there as long as after. |
| `items.after` | `10s` |  |
| `creatures.burn` | `true` | Whether a creature in the sea is hurt, by damage at each check. A heart is 2. |
| `creatures.damage` | `2.0` |  |
| `creatures.spared` | `drowned`, `guardian`, `elder_guardian` | The creatures, by type, the sea does not hurt: those that live in it. |

## `clean-water`

What a bottle of clean water drunk on an AcidIsland island does. heals is the health it gives back, up to the drinker's most, 0 for none; a heart is 2. effects are given too, each written name:amplifier:seconds, as "regeneration:0:5".

| Key | Default | What it does |
|---|---|---|
| `heals` | `4.0` |  |
| `effects` | empty list |  |

## `purification`

How a bottle of acid water is made clean.

| Key | Default | What it does |
|---|---|---|
| `furnace` | `true` | Smelting it in a furnace. A bucket of water smelted comes out clean too, and poured into a cauldron keeps it clean. |
| `brewing-with-coal` | `true` | Brewing it with coal in a brewing stand. |
