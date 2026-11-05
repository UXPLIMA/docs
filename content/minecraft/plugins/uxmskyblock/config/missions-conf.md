---
title: missions.conf
order: 632
description: "Island missions: daily, weekly and challenges, and their pay."
icon: target
---

Island missions: what an island is asked to do, and what it is paid.

The file is `plugins/uxmSkyblock/modules/missions.conf`. Every default below is the shipped value.

## `missions`

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` |  |
| `reset-zone` | `system` | The time zone a mission's day and week turn in, as "Europe/Istanbul" or "UTC". "system" is the zone the server runs in. |
| `daily-slots` | `0` | How many daily missions each island is offered a day, drawn from every daily mission its mode plays and drawn again when the day turns. Every island draws its own, and the same all day, on every server. 0 offers every daily mission. |

## `missions.catalog`

Every mission takes repeat: "once" is finished once and stays finished, which is what the missions window calls a challenge. "daily" starts again at midnight and "weekly" at midnight between Sunday and Monday, and each pays again every time it is finished. A mission that leaves repeat out is finished once. Only a mission finished once counts towards the island level, which would otherwise fall every time a day or a week turned.

trigger-type is what a mission counts, target-filter what it counts it for ("*" or left out for anything), and required-amount how many:

```text
BLOCK_BREAK, BLOCK_PLACE    a block, by its name, as STONE
CROP_HARVEST                a crop broken ripe, as WHEAT
MOB_KILL                    a creature killed, as ZOMBIE
FISHING                     a catch
CRAFT                       an item crafted, as BREAD
SMELT                       an item taken out of a furnace, as IRON_INGOT
ENCHANT                     an item enchanted, as DIAMOND_SWORD
BREW                        a potion brewed at a stand the player opened,
                            by its kind, as SWIFTNESS
EXPERIENCE                  experience points gained
ITEM_SUBMIT                 items handed in from the missions window
ONEBLOCK_BREAK              a OneBlock island's block broken, by the phase it
                            was broken in
ONEBLOCK_PHASE              a OneBlock island reaching a phase, by its key
BOXED_GROW                  a Boxed island's box growing, by how far
LEVEL_REACH                 the island reaching a level; required-amount is
                            the level
BANK_BALANCE                the island bank holding a balance, in whole units
                            of the server's money
```

A mission may ask for several things at once. Write lines = [ ... ] in place of trigger-type, target-filter and required-amount, one `( )` for each, as

```text
lines = [
    { trigger-type = "BLOCK_BREAK", target-filter = "STONE", required-amount = 64 }
    { trigger-type = "MOB_KILL", target-filter = "ZOMBIE", required-amount = 10 }
]
```

Each line is counted on its own, and the mission is finished, and pays, when every line is. A line cannot count ITEM_SUBMIT.

A mission may name the modes it belongs to, as modes = ["oneblock", "boxed"]: only an island playing one of them is offered it and moves it on. A mission that names none belongs to every mode. A mode no mode is called leaves the mission out, with a line in the console. The modes are skyblock, oneblock, chunkblock, acid_island, caveblock, skygrid, boxed, poseidon, stranger_realms, parkour, brix and tradewinds.

A mission may follow another, as after = "farming_wheat_1": it is offered once that one is finished, which makes a chain of missions one after another.

## `missions.catalog.oneblock_phase_underground`

The packs of the modes. Each is offered only to an island playing its mode, and each chain one mission after another.

| Key | Default | What it does |
|---|---|---|
| `branch` | `ADVENTURE` |  |
| `display-name` | `@missions.catalog.oneblock_phase_underground.name` |  |
| `description` | `@missions.catalog.oneblock_phase_underground.description` |  |
| `trigger-type` | `ONEBLOCK_PHASE` |  |
| `target-filter` | `underground` |  |
| `required-amount` | `1` |  |
| `repeat` | `once` |  |
| `modes` | `oneblock` |  |
| `rewards.crystals` | `10` |  |
| `rewards.currency-minor-units` | `1000` |  |
| `rewards.island-exp` | `150` |  |
| `rewards.commands` | empty list |  |
| `missions.catalog.oneblock_phase_snow.branch` | `ADVENTURE` |  |
| `missions.catalog.oneblock_phase_snow.display-name` | `@missions.catalog.oneblock_phase_snow.name` |  |
| `missions.catalog.oneblock_phase_snow.description` | `@missions.catalog.oneblock_phase_snow.description` |  |
| `missions.catalog.oneblock_phase_snow.trigger-type` | `ONEBLOCK_PHASE` |  |
| `missions.catalog.oneblock_phase_snow.target-filter` | `snow` |  |
| `missions.catalog.oneblock_phase_snow.required-amount` | `1` |  |
| `missions.catalog.oneblock_phase_snow.repeat` | `once` |  |
| `missions.catalog.oneblock_phase_snow.modes` | `oneblock` |  |
| `missions.catalog.oneblock_phase_snow.after` | `oneblock_phase_underground` |  |
| `missions.catalog.oneblock_phase_snow.rewards.crystals` | `15` |  |
| `missions.catalog.oneblock_phase_snow.rewards.currency-minor-units` | `1500` |  |
| `missions.catalog.oneblock_phase_snow.rewards.island-exp` | `200` |  |
| `missions.catalog.oneblock_phase_snow.rewards.commands` | empty list |  |
| `missions.catalog.oneblock_phase_ocean.branch` | `ADVENTURE` |  |
| `missions.catalog.oneblock_phase_ocean.display-name` | `@missions.catalog.oneblock_phase_ocean.name` |  |
| `missions.catalog.oneblock_phase_ocean.description` | `@missions.catalog.oneblock_phase_ocean.description` |  |
| `missions.catalog.oneblock_phase_ocean.trigger-type` | `ONEBLOCK_PHASE` |  |
| `missions.catalog.oneblock_phase_ocean.target-filter` | `ocean` |  |
| `missions.catalog.oneblock_phase_ocean.required-amount` | `1` |  |
| `missions.catalog.oneblock_phase_ocean.repeat` | `once` |  |
| `missions.catalog.oneblock_phase_ocean.modes` | `oneblock` |  |
| `missions.catalog.oneblock_phase_ocean.after` | `oneblock_phase_snow` |  |
| `missions.catalog.oneblock_phase_ocean.rewards.crystals` | `20` |  |
| `missions.catalog.oneblock_phase_ocean.rewards.currency-minor-units` | `2000` |  |
| `missions.catalog.oneblock_phase_ocean.rewards.island-exp` | `250` |  |
| `missions.catalog.oneblock_phase_ocean.rewards.commands` | empty list |  |
| `missions.catalog.oneblock_phase_jungle.branch` | `ADVENTURE` |  |
| `missions.catalog.oneblock_phase_jungle.display-name` | `@missions.catalog.oneblock_phase_jungle.name` |  |
| `missions.catalog.oneblock_phase_jungle.description` | `@missions.catalog.oneblock_phase_jungle.description` |  |
| `missions.catalog.oneblock_phase_jungle.trigger-type` | `ONEBLOCK_PHASE` |  |
| `missions.catalog.oneblock_phase_jungle.target-filter` | `jungle` |  |
| `missions.catalog.oneblock_phase_jungle.required-amount` | `1` |  |
| `missions.catalog.oneblock_phase_jungle.repeat` | `once` |  |
| `missions.catalog.oneblock_phase_jungle.modes` | `oneblock` |  |
| `missions.catalog.oneblock_phase_jungle.after` | `oneblock_phase_ocean` |  |
| `missions.catalog.oneblock_phase_jungle.rewards.crystals` | `25` |  |
| `missions.catalog.oneblock_phase_jungle.rewards.currency-minor-units` | `2500` |  |
| `missions.catalog.oneblock_phase_jungle.rewards.island-exp` | `300` |  |
| `missions.catalog.oneblock_phase_jungle.rewards.commands` | empty list |  |
| `missions.catalog.oneblock_phase_nether.branch` | `ADVENTURE` |  |
| `missions.catalog.oneblock_phase_nether.display-name` | `@missions.catalog.oneblock_phase_nether.name` |  |
| `missions.catalog.oneblock_phase_nether.description` | `@missions.catalog.oneblock_phase_nether.description` |  |
| `missions.catalog.oneblock_phase_nether.trigger-type` | `ONEBLOCK_PHASE` |  |
| `missions.catalog.oneblock_phase_nether.target-filter` | `nether` |  |
| `missions.catalog.oneblock_phase_nether.required-amount` | `1` |  |
| `missions.catalog.oneblock_phase_nether.repeat` | `once` |  |
| `missions.catalog.oneblock_phase_nether.modes` | `oneblock` |  |
| `missions.catalog.oneblock_phase_nether.after` | `oneblock_phase_jungle` |  |
| `missions.catalog.oneblock_phase_nether.rewards.crystals` | `35` |  |
| `missions.catalog.oneblock_phase_nether.rewards.currency-minor-units` | `3500` |  |
| `missions.catalog.oneblock_phase_nether.rewards.island-exp` | `400` |  |
| `missions.catalog.oneblock_phase_nether.rewards.commands` | empty list |  |
| `missions.catalog.oneblock_phase_end.branch` | `ADVENTURE` |  |
| `missions.catalog.oneblock_phase_end.display-name` | `@missions.catalog.oneblock_phase_end.name` |  |
| `missions.catalog.oneblock_phase_end.description` | `@missions.catalog.oneblock_phase_end.description` |  |
| `missions.catalog.oneblock_phase_end.trigger-type` | `ONEBLOCK_PHASE` |  |
| `missions.catalog.oneblock_phase_end.target-filter` | `end` |  |
| `missions.catalog.oneblock_phase_end.required-amount` | `1` |  |
| `missions.catalog.oneblock_phase_end.repeat` | `once` |  |
| `missions.catalog.oneblock_phase_end.modes` | `oneblock` |  |
| `missions.catalog.oneblock_phase_end.after` | `oneblock_phase_nether` |  |
| `missions.catalog.oneblock_phase_end.rewards.crystals` | `50` |  |
| `missions.catalog.oneblock_phase_end.rewards.currency-minor-units` | `5000` |  |
| `missions.catalog.oneblock_phase_end.rewards.island-exp` | `600` |  |
| `missions.catalog.oneblock_phase_end.rewards.commands` | empty list |  |
| `missions.catalog.oneblock_daily_breaks.branch` | `MINING` |  |
| `missions.catalog.oneblock_daily_breaks.display-name` | `@missions.catalog.oneblock_daily_breaks.name` |  |
| `missions.catalog.oneblock_daily_breaks.description` | `@missions.catalog.oneblock_daily_breaks.description` |  |
| `missions.catalog.oneblock_daily_breaks.trigger-type` | `ONEBLOCK_BREAK` |  |
| `missions.catalog.oneblock_daily_breaks.target-filter` | `*` |  |
| `missions.catalog.oneblock_daily_breaks.required-amount` | `200` |  |
| `missions.catalog.oneblock_daily_breaks.repeat` | `daily` |  |
| `missions.catalog.oneblock_daily_breaks.modes` | `oneblock` |  |
| `missions.catalog.oneblock_daily_breaks.rewards.crystals` | `5` |  |
| `missions.catalog.oneblock_daily_breaks.rewards.currency-minor-units` | `300` |  |
| `missions.catalog.oneblock_daily_breaks.rewards.island-exp` | `60` |  |
| `missions.catalog.oneblock_daily_breaks.rewards.commands` | empty list |  |
| `missions.catalog.boxed_grow_1.branch` | `ADVENTURE` |  |
| `missions.catalog.boxed_grow_1.display-name` | `@missions.catalog.boxed_grow_1.name` |  |
| `missions.catalog.boxed_grow_1.description` | `@missions.catalog.boxed_grow_1.description` |  |
| `missions.catalog.boxed_grow_1.trigger-type` | `BOXED_GROW` |  |
| `missions.catalog.boxed_grow_1.target-filter` | `*` |  |
| `missions.catalog.boxed_grow_1.required-amount` | `10` |  |
| `missions.catalog.boxed_grow_1.repeat` | `once` |  |
| `missions.catalog.boxed_grow_1.modes` | `boxed` |  |
| `missions.catalog.boxed_grow_1.rewards.crystals` | `10` |  |
| `missions.catalog.boxed_grow_1.rewards.currency-minor-units` | `1000` |  |
| `missions.catalog.boxed_grow_1.rewards.island-exp` | `150` |  |
| `missions.catalog.boxed_grow_1.rewards.commands` | empty list |  |
| `missions.catalog.boxed_grow_2.branch` | `ADVENTURE` |  |
| `missions.catalog.boxed_grow_2.display-name` | `@missions.catalog.boxed_grow_2.name` |  |
| `missions.catalog.boxed_grow_2.description` | `@missions.catalog.boxed_grow_2.description` |  |
| `missions.catalog.boxed_grow_2.trigger-type` | `BOXED_GROW` |  |
| `missions.catalog.boxed_grow_2.target-filter` | `*` |  |
| `missions.catalog.boxed_grow_2.required-amount` | `25` |  |
| `missions.catalog.boxed_grow_2.repeat` | `once` |  |
| `missions.catalog.boxed_grow_2.modes` | `boxed` |  |
| `missions.catalog.boxed_grow_2.after` | `boxed_grow_1` |  |
| `missions.catalog.boxed_grow_2.rewards.crystals` | `20` |  |
| `missions.catalog.boxed_grow_2.rewards.currency-minor-units` | `2000` |  |
| `missions.catalog.boxed_grow_2.rewards.island-exp` | `300` |  |
| `missions.catalog.boxed_grow_2.rewards.commands` | empty list |  |
| `missions.catalog.boxed_grow_3.branch` | `ADVENTURE` |  |
| `missions.catalog.boxed_grow_3.display-name` | `@missions.catalog.boxed_grow_3.name` |  |
| `missions.catalog.boxed_grow_3.description` | `@missions.catalog.boxed_grow_3.description` |  |
| `missions.catalog.boxed_grow_3.trigger-type` | `BOXED_GROW` |  |
| `missions.catalog.boxed_grow_3.target-filter` | `*` |  |
| `missions.catalog.boxed_grow_3.required-amount` | `60` |  |
| `missions.catalog.boxed_grow_3.repeat` | `once` |  |
| `missions.catalog.boxed_grow_3.modes` | `boxed` |  |
| `missions.catalog.boxed_grow_3.after` | `boxed_grow_2` |  |
| `missions.catalog.boxed_grow_3.rewards.crystals` | `40` |  |
| `missions.catalog.boxed_grow_3.rewards.currency-minor-units` | `4000` |  |
| `missions.catalog.boxed_grow_3.rewards.island-exp` | `500` |  |
| `missions.catalog.boxed_grow_3.rewards.commands` | empty list |  |
| `missions.catalog.poseidon_story_1.branch` | `ADVENTURE` |  |
| `missions.catalog.poseidon_story_1.display-name` | `@missions.catalog.poseidon_story_1.name` |  |
| `missions.catalog.poseidon_story_1.description` | `@missions.catalog.poseidon_story_1.description` |  |
| `missions.catalog.poseidon_story_1.trigger-type` | `ITEM_SUBMIT` |  |
| `missions.catalog.poseidon_story_1.target-filter` | `PRISMARINE_SHARD` |  |
| `missions.catalog.poseidon_story_1.required-amount` | `16` |  |
| `missions.catalog.poseidon_story_1.repeat` | `once` |  |
| `missions.catalog.poseidon_story_1.modes` | `poseidon` |  |
| `missions.catalog.poseidon_story_1.rewards.crystals` | `10` |  |
| `missions.catalog.poseidon_story_1.rewards.currency-minor-units` | `1000` |  |
| `missions.catalog.poseidon_story_1.rewards.island-exp` | `150` |  |
| `missions.catalog.poseidon_story_1.rewards.commands` | empty list |  |
| `missions.catalog.poseidon_story_2.branch` | `SLAYER` |  |
| `missions.catalog.poseidon_story_2.display-name` | `@missions.catalog.poseidon_story_2.name` |  |
| `missions.catalog.poseidon_story_2.description` | `@missions.catalog.poseidon_story_2.description` |  |
| `missions.catalog.poseidon_story_2.trigger-type` | `MOB_KILL` |  |
| `missions.catalog.poseidon_story_2.target-filter` | `GUARDIAN` |  |
| `missions.catalog.poseidon_story_2.required-amount` | `10` |  |
| `missions.catalog.poseidon_story_2.repeat` | `once` |  |
| `missions.catalog.poseidon_story_2.modes` | `poseidon` |  |
| `missions.catalog.poseidon_story_2.after` | `poseidon_story_1` |  |
| `missions.catalog.poseidon_story_2.rewards.crystals` | `15` |  |
| `missions.catalog.poseidon_story_2.rewards.currency-minor-units` | `1500` |  |
| `missions.catalog.poseidon_story_2.rewards.island-exp` | `200` |  |
| `missions.catalog.poseidon_story_2.rewards.commands` | empty list |  |
| `missions.catalog.poseidon_story_3.branch` | `BUILDER` |  |
| `missions.catalog.poseidon_story_3.display-name` | `@missions.catalog.poseidon_story_3.name` |  |
| `missions.catalog.poseidon_story_3.description` | `@missions.catalog.poseidon_story_3.description` |  |
| `missions.catalog.poseidon_story_3.trigger-type` | `BLOCK_PLACE` |  |
| `missions.catalog.poseidon_story_3.target-filter` | `SEA_LANTERN` |  |
| `missions.catalog.poseidon_story_3.required-amount` | `8` |  |
| `missions.catalog.poseidon_story_3.repeat` | `once` |  |
| `missions.catalog.poseidon_story_3.modes` | `poseidon` |  |
| `missions.catalog.poseidon_story_3.after` | `poseidon_story_2` |  |
| `missions.catalog.poseidon_story_3.rewards.crystals` | `20` |  |
| `missions.catalog.poseidon_story_3.rewards.currency-minor-units` | `2000` |  |
| `missions.catalog.poseidon_story_3.rewards.island-exp` | `250` |  |
| `missions.catalog.poseidon_story_3.rewards.commands` | empty list |  |
| `missions.catalog.poseidon_story_4.branch` | `ADVENTURE` |  |
| `missions.catalog.poseidon_story_4.display-name` | `@missions.catalog.poseidon_story_4.name` |  |
| `missions.catalog.poseidon_story_4.description` | `@missions.catalog.poseidon_story_4.description` |  |
| `missions.catalog.poseidon_story_4.trigger-type` | `FISHING` |  |
| `missions.catalog.poseidon_story_4.target-filter` | `*` |  |
| `missions.catalog.poseidon_story_4.required-amount` | `30` |  |
| `missions.catalog.poseidon_story_4.repeat` | `once` |  |
| `missions.catalog.poseidon_story_4.modes` | `poseidon` |  |
| `missions.catalog.poseidon_story_4.after` | `poseidon_story_3` |  |
| `missions.catalog.poseidon_story_4.rewards.crystals` | `25` |  |
| `missions.catalog.poseidon_story_4.rewards.currency-minor-units` | `2500` |  |
| `missions.catalog.poseidon_story_4.rewards.island-exp` | `300` |  |
| `missions.catalog.poseidon_story_4.rewards.commands` | empty list |  |
| `missions.catalog.poseidon_story_5.branch` | `SLAYER` |  |
| `missions.catalog.poseidon_story_5.display-name` | `@missions.catalog.poseidon_story_5.name` |  |
| `missions.catalog.poseidon_story_5.description` | `@missions.catalog.poseidon_story_5.description` |  |
| `missions.catalog.poseidon_story_5.trigger-type` | `MOB_KILL` |  |
| `missions.catalog.poseidon_story_5.target-filter` | `ELDER_GUARDIAN` |  |
| `missions.catalog.poseidon_story_5.required-amount` | `1` |  |
| `missions.catalog.poseidon_story_5.repeat` | `once` |  |
| `missions.catalog.poseidon_story_5.modes` | `poseidon` |  |
| `missions.catalog.poseidon_story_5.after` | `poseidon_story_4` |  |
| `missions.catalog.poseidon_story_5.rewards.crystals` | `60` |  |
| `missions.catalog.poseidon_story_5.rewards.currency-minor-units` | `6000` |  |
| `missions.catalog.poseidon_story_5.rewards.island-exp` | `800` |  |
| `missions.catalog.poseidon_story_5.rewards.commands` | empty list |  |
| `missions.catalog.farming_wheat_1.branch` | `FARMING` |  |
| `missions.catalog.farming_wheat_1.display-name` | `@missions.catalog.farming_wheat_1.name` |  |
| `missions.catalog.farming_wheat_1.description` | `@missions.catalog.farming_wheat_1.description` |  |
| `missions.catalog.farming_wheat_1.trigger-type` | `CROP_HARVEST` |  |
| `missions.catalog.farming_wheat_1.target-filter` | `WHEAT` |  |
| `missions.catalog.farming_wheat_1.required-amount` | `50` |  |
| `missions.catalog.farming_wheat_1.rewards.crystals` | `10` |  |
| `missions.catalog.farming_wheat_1.rewards.currency-minor-units` | `500` |  |
| `missions.catalog.farming_wheat_1.rewards.island-exp` | `100` |  |
| `missions.catalog.farming_wheat_1.rewards.commands` | empty list |  |
| `missions.catalog.farming_carrot_1.branch` | `FARMING` |  |
| `missions.catalog.farming_carrot_1.display-name` | `@missions.catalog.farming_carrot_1.name` |  |
| `missions.catalog.farming_carrot_1.description` | `@missions.catalog.farming_carrot_1.description` |  |
| `missions.catalog.farming_carrot_1.trigger-type` | `CROP_HARVEST` |  |
| `missions.catalog.farming_carrot_1.target-filter` | `CARROT` |  |
| `missions.catalog.farming_carrot_1.required-amount` | `100` |  |
| `missions.catalog.farming_carrot_1.rewards.crystals` | `15` |  |
| `missions.catalog.farming_carrot_1.rewards.currency-minor-units` | `1000` |  |
| `missions.catalog.farming_carrot_1.rewards.island-exp` | `150` |  |
| `missions.catalog.farming_carrot_1.rewards.commands` | empty list |  |
| `missions.catalog.mining_stone_1.branch` | `MINING` |  |
| `missions.catalog.mining_stone_1.display-name` | `@missions.catalog.mining_stone_1.name` |  |
| `missions.catalog.mining_stone_1.description` | `@missions.catalog.mining_stone_1.description` |  |
| `missions.catalog.mining_stone_1.trigger-type` | `BLOCK_BREAK` |  |
| `missions.catalog.mining_stone_1.target-filter` | `STONE` |  |
| `missions.catalog.mining_stone_1.required-amount` | `128` |  |
| `missions.catalog.mining_stone_1.rewards.crystals` | `10` |  |
| `missions.catalog.mining_stone_1.rewards.currency-minor-units` | `400` |  |
| `missions.catalog.mining_stone_1.rewards.island-exp` | `100` |  |
| `missions.catalog.mining_stone_1.rewards.commands` | empty list |  |
| `missions.catalog.slayer_zombie_1.branch` | `SLAYER` |  |
| `missions.catalog.slayer_zombie_1.display-name` | `@missions.catalog.slayer_zombie_1.name` |  |
| `missions.catalog.slayer_zombie_1.description` | `@missions.catalog.slayer_zombie_1.description` |  |
| `missions.catalog.slayer_zombie_1.trigger-type` | `MOB_KILL` |  |
| `missions.catalog.slayer_zombie_1.target-filter` | `ZOMBIE` |  |
| `missions.catalog.slayer_zombie_1.required-amount` | `25` |  |
| `missions.catalog.slayer_zombie_1.rewards.crystals` | `20` |  |
| `missions.catalog.slayer_zombie_1.rewards.currency-minor-units` | `1500` |  |
| `missions.catalog.slayer_zombie_1.rewards.island-exp` | `250` |  |
| `missions.catalog.slayer_zombie_1.rewards.commands` | empty list |  |
| `missions.catalog.builder_cobble_1.branch` | `BUILDER` |  |
| `missions.catalog.builder_cobble_1.display-name` | `@missions.catalog.builder_cobble_1.name` |  |
| `missions.catalog.builder_cobble_1.description` | `@missions.catalog.builder_cobble_1.description` |  |
| `missions.catalog.builder_cobble_1.trigger-type` | `BLOCK_PLACE` |  |
| `missions.catalog.builder_cobble_1.target-filter` | `COBBLESTONE` |  |
| `missions.catalog.builder_cobble_1.required-amount` | `256` |  |
| `missions.catalog.builder_cobble_1.rewards.crystals` | `15` |  |
| `missions.catalog.builder_cobble_1.rewards.currency-minor-units` | `800` |  |
| `missions.catalog.builder_cobble_1.rewards.island-exp` | `150` |  |
| `missions.catalog.builder_cobble_1.rewards.commands` | empty list |  |
| `missions.catalog.adventure_fishing_1.branch` | `ADVENTURE` |  |
| `missions.catalog.adventure_fishing_1.display-name` | `@missions.catalog.adventure_fishing_1.name` |  |
| `missions.catalog.adventure_fishing_1.description` | `@missions.catalog.adventure_fishing_1.description` |  |
| `missions.catalog.adventure_fishing_1.trigger-type` | `FISHING` |  |
| `missions.catalog.adventure_fishing_1.target-filter` | `*` |  |
| `missions.catalog.adventure_fishing_1.required-amount` | `20` |  |
| `missions.catalog.adventure_fishing_1.rewards.crystals` | `25` |  |
| `missions.catalog.adventure_fishing_1.rewards.currency-minor-units` | `2000` |  |
| `missions.catalog.adventure_fishing_1.rewards.island-exp` | `300` |  |
| `missions.catalog.adventure_fishing_1.rewards.commands` | empty list |  |
| `missions.catalog.economy_iron_1.branch` | `ECONOMY` |  |
| `missions.catalog.economy_iron_1.display-name` | `@missions.catalog.economy_iron_1.name` |  |
| `missions.catalog.economy_iron_1.description` | `@missions.catalog.economy_iron_1.description` |  |
| `missions.catalog.economy_iron_1.trigger-type` | `ITEM_SUBMIT` |  |
| `missions.catalog.economy_iron_1.target-filter` | `IRON_INGOT` |  |
| `missions.catalog.economy_iron_1.required-amount` | `64` |  |
| `missions.catalog.economy_iron_1.rewards.crystals` | `50` |  |
| `missions.catalog.economy_iron_1.rewards.currency-minor-units` | `5000` |  |
| `missions.catalog.economy_iron_1.rewards.island-exp` | `1000` |  |
| `missions.catalog.economy_iron_1.rewards.commands` | empty list |  |
| `missions.catalog.daily_cobblestone.branch` | `MINING` |  |
| `missions.catalog.daily_cobblestone.display-name` | `@missions.catalog.daily_cobblestone.name` |  |
| `missions.catalog.daily_cobblestone.description` | `@missions.catalog.daily_cobblestone.description` |  |
| `missions.catalog.daily_cobblestone.trigger-type` | `BLOCK_BREAK` |  |
| `missions.catalog.daily_cobblestone.target-filter` | `COBBLESTONE` |  |
| `missions.catalog.daily_cobblestone.required-amount` | `256` |  |
| `missions.catalog.daily_cobblestone.repeat` | `daily` |  |
| `missions.catalog.daily_cobblestone.rewards.crystals` | `5` |  |
| `missions.catalog.daily_cobblestone.rewards.currency-minor-units` | `250` |  |
| `missions.catalog.daily_cobblestone.rewards.island-exp` | `50` |  |
| `missions.catalog.daily_cobblestone.rewards.commands` | empty list |  |
| `missions.catalog.daily_wheat.branch` | `FARMING` |  |
| `missions.catalog.daily_wheat.display-name` | `@missions.catalog.daily_wheat.name` |  |
| `missions.catalog.daily_wheat.description` | `@missions.catalog.daily_wheat.description` |  |
| `missions.catalog.daily_wheat.trigger-type` | `CROP_HARVEST` |  |
| `missions.catalog.daily_wheat.target-filter` | `WHEAT` |  |
| `missions.catalog.daily_wheat.required-amount` | `64` |  |
| `missions.catalog.daily_wheat.repeat` | `daily` |  |
| `missions.catalog.daily_wheat.rewards.crystals` | `5` |  |
| `missions.catalog.daily_wheat.rewards.currency-minor-units` | `250` |  |
| `missions.catalog.daily_wheat.rewards.island-exp` | `50` |  |
| `missions.catalog.daily_wheat.rewards.commands` | empty list |  |
| `missions.catalog.weekly_hunter.branch` | `SLAYER` |  |
| `missions.catalog.weekly_hunter.display-name` | `@missions.catalog.weekly_hunter.name` |  |
| `missions.catalog.weekly_hunter.description` | `@missions.catalog.weekly_hunter.description` |  |
| `missions.catalog.weekly_hunter.trigger-type` | `MOB_KILL` |  |
| `missions.catalog.weekly_hunter.target-filter` | `*` |  |
| `missions.catalog.weekly_hunter.required-amount` | `100` |  |
| `missions.catalog.weekly_hunter.repeat` | `weekly` |  |
| `missions.catalog.weekly_hunter.rewards.crystals` | `30` |  |
| `missions.catalog.weekly_hunter.rewards.currency-minor-units` | `2500` |  |
| `missions.catalog.weekly_hunter.rewards.island-exp` | `400` |  |
| `missions.catalog.weekly_hunter.rewards.commands` | empty list |  |
| `missions.catalog.weekly_angler.branch` | `ADVENTURE` |  |
| `missions.catalog.weekly_angler.display-name` | `@missions.catalog.weekly_angler.name` |  |
| `missions.catalog.weekly_angler.description` | `@missions.catalog.weekly_angler.description` |  |
| `missions.catalog.weekly_angler.trigger-type` | `FISHING` |  |
| `missions.catalog.weekly_angler.target-filter` | `*` |  |
| `missions.catalog.weekly_angler.required-amount` | `50` |  |
| `missions.catalog.weekly_angler.repeat` | `weekly` |  |
| `missions.catalog.weekly_angler.rewards.crystals` | `25` |  |
| `missions.catalog.weekly_angler.rewards.currency-minor-units` | `2000` |  |
| `missions.catalog.weekly_angler.rewards.island-exp` | `300` |  |
| `missions.catalog.weekly_angler.rewards.commands` | empty list |  |
