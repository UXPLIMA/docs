---
title: poseidon.conf
order: 637
description: "Poseidon: the sea floor island, breath, lore and the deep boss."
icon: waves
---

Poseidon: an island on the sea floor. Water is the air a Poseidon player breathes: in it they see and breathe, out of it dry air and the sun burn them, and one who stops moving in the water drowns. The Nether of a Poseidon island is flooded.

The file is `plugins/uxmSkyblock/modules/poseidon.conf`. Every default below is the shipped value.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Whether islands can be made as Poseidon islands. |

## `ocean`

The ocean an island lies at the bottom of. It fills the air around the island and never replaces a block the island already has.

| Key | Default | What it does |
|---|---|---|
| `above` | `24` | How many blocks of water stand over the block players arrive on. |
| `depth` | `8` | How many blocks of water lie under it, down to the floor. |
| `radius` | `64` | How far from the island's centre the ocean reaches. Keep it inside half the grid spacing, so two islands' oceans never meet. |
| `floor` | `SANDSTONE` | The block the ocean stands on. A block that falls, such as sand or gravel, would drop out from under the ocean and let it drain away, so one of those is replaced by sandstone. |

## `wrecks`

Wrecks and ruins laid on the sea floor around a Poseidon island. Each entry is a creation action a preset names as "uxm:`<name>`": "uxm:shipwreck", "uxm:ruin" and "uxm:ruins" here. An entry may be added under a name of your own. A template only fills water and air, so it never breaks into the island.

## `wrecks.shipwreck`

| Key | Default | What it does |
|---|---|---|
| `templates` | a list of 4 | The structure templates to choose from: the game's own, or any a datapack adds. |
| `count` | `1` | How many are laid, spread evenly around the island. |
| `distance` | `22` | How far from the island's centre each one lies. Keep it inside the ocean's radius. |
| `loot` | `minecraft:chests/shipwreck_supply`, `minecraft:chests/shipwreck_treasure`, `minecraft:chests/shipwreck_map` | The loot tables the chests get, each in turn. |
| `wrecks.ruin.templates` | a list of 4 |  |
| `wrecks.ruin.count` | `1` |  |
| `wrecks.ruin.distance` | `40` |  |
| `wrecks.ruin.loot` | `minecraft:chests/underwater_ruin_big` |  |
| `wrecks.ruins.templates` | a list of 8 |  |
| `wrecks.ruins.count` | `5` |  |
| `wrecks.ruins.distance` | `44` |  |
| `wrecks.ruins.loot` | `minecraft:chests/underwater_ruin_small` |  |

## `monument`

The sunken temple a monument start lays right under the island, as "uxm:monument": the island stands on its roof, and a swimmer comes in by the door on each side. It is laid as a wreck is. A template named schematic:`<name>` is the file schematics/`<name>`.schem under the plugin's folder: the plugin writes schematics/monument.schem on its first start, and you may edit it with any tool that writes a Sponge schematic, or with /is schematic save. Keep its height below the ocean's depth, or the island stands on its roof. A wreck group of your own called monument takes its place.

| Key | Default | What it does |
|---|---|---|
| `templates` | `schematic:monument` |  |
| `count` | `1` |  |
| `distance` | `0` |  |
| `loot` | `minecraft:chests/underwater_ruin_big` |  |

## `hazard`

How the air hurts a player on a Poseidon island. Water is their air: in it nothing hurts, unless they stay in one spot too long. A player in creative or spectator is never touched.

| Key | Default | What it does |
|---|---|---|
| `check-every` | `1s` | How often each player on a Poseidon island is checked. |
| `dry-damage` | `1.0` | Health taken at each check from a player out of the water. A heart is 2. |
| `sun-damage` | `2.0` | Health the sun adds at each check, on a player out of the water under an open daytime sky. |
| `still-damage` | `2.0` | Health taken at each check from a swimmer who has stayed still too long. |
| `still-after` | `5s` | How long a swimmer may stay still before the water hurts. |
| `still-reach` | `1.0` | How far a swimmer must move, in blocks, to count as moving. |
| `rain-is-wet` | `true` | Whether rain keeps a player out of the water from drying. |
| `water-effects` | `water_breathing:0:4`, `night_vision:0:15` | Effects the water gives, each written name:amplifier:seconds. Keep each longer than check-every, or it runs out between two checks. |

## `lore`

The lore book (/is lore) follows the story. After the pages that open it come the chapters of these missions, in this order: a chapter is the lines of poseidon.lore.chapters.`<mission>` in the reader's language file, shown once the reader has finished that mission. The first chapter not yet earned is a page that names its mission, and nothing after it shows. A mission the missions file does not hold is skipped. Leave the list empty for a book without a story.

| Key | Default | What it does |
|---|---|---|
| `chapters` | `poseidon_story_1`, `poseidon_story_2`, `poseidon_story_3`, `poseidon_story_4`, `poseidon_story_5` |  |

## `boss`

The deep boss. Now and then, while a player swims on a Poseidon island, something rises from the deep beside them. Each look at a player in the water rolls the chance; one island sees at most one boss at a time, and none again until every has passed since the last. Whoever slays it is given the loot table named here, from loot/, besides what the creature drops of itself. If nobody slays it, it sinks back after lasts. The effect lists poseidon-boss-rises, poseidon-boss-slain and poseidon-boss-sinks of modules/effects.conf fire for every player on the island.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` |  |
| `entity` | `elder_guardian` | The creature that rises, by its key. |
| `health` | `160` |  |
| `check-every` | `1m` |  |
| `chance` | `0.05` | From 0 to 1. |
| `every` | `2h` |  |
| `lasts` | `10m` |  |
| `loot` | `deep_boss` | A table in loot/, by its name; leave it empty for none. |

## `coral`

Coral spreads slowly on a Poseidon island. Around each player there, at each look, samples blocks within radius of them are looked at; a living coral block among them grows, by chance, a coral or a fan of its colour into the water above it, or a fan onto its side. Plant coral blocks to grow a reef.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` |  |
| `check-every` | `30s` |  |
| `samples` | `24` |  |
| `radius` | `12` |  |
| `chance` | `0.25` | From 0 to 1. |
