---
title: upgrades.conf
order: 651
description: "Island upgrades: every upgrade, its tiers and their prices."
icon: arrow-up-circle
---

The file is `plugins/uxmSkyblock/modules/upgrades.conf`. Every default below is the shipped value.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Island upgrades: what an island buys with its bank, a tier at a time, from /is upgrades. Every upgrade below has tiers, bought in the order they are written. A tier takes: cost what it costs, in hundredths of the island's money: 50000 is 500.00. The first tier of an upgrade that costs 0 is where every island starts. currency what it is paid in. "PRIMARY" is the island's own money, the bank's money. properties what the tier sets, read by the upgrade it belongs to: radius and size for island_size, max_members for member_limit, limit for bank_limit, and so on. display-name is the upgrade's name, a key of the language files written with a leading @, or your own words. A purchase asks the island role permission to take money from the bank, and whatever required-permission an upgrade names. Add a tier by writing one more entry at the end. |
| `upgrades.island_size.display-name` | `@upgrades.names.island_size` |  |
| `upgrades.island_size.tiers` | a list of 5 entries, below |  |
| `upgrades.member_limit.display-name` | `@upgrades.names.member_limit` |  |
| `upgrades.member_limit.tiers` | a list of 4 entries, below |  |
| `upgrades.ore_generator.display-name` | `@upgrades.names.ore_generator` |  |
| `upgrades.ore_generator.tiers` | a list of 4 entries, below |  |
| `upgrades.crop_growth.display-name` | `@upgrades.names.crop_growth` |  |
| `upgrades.crop_growth.tiers` | a list of 3 entries, below |  |
| `upgrades.warp_slots.display-name` | `@upgrades.names.warp_slots` |  |
| `upgrades.warp_slots.tiers` | a list of 3 entries, below |  |
| `upgrades.vault_pages.display-name` | `@upgrades.names.vault_pages` |  |
| `upgrades.vault_pages.tiers` | a list of 3 entries, below |  |
| `upgrades.spawner_rates.display-name` | `@upgrades.names.spawner_rates` |  |
| `upgrades.spawner_rates.required-permission` | `SPAWNER_UPGRADE` | The role permission this upgrade asks for, beyond the withdraw permission every purchase asks for because it spends the island bank. Name any permission the role editor knows, or leave it out and the money is the only question. |
| `upgrades.spawner_rates.tiers` | a list of 3 entries, below |  |

`upgrades.island_size.tiers`:

| # | `cost` | `currency` | `properties.radius` | `properties.size` |
|---|---|---|---|---|
| 1 | `0` | `PRIMARY` | `25.0` | `50.0` |
| 2 | `50000` | `PRIMARY` | `37.5` | `75.0` |
| 3 | `150000` | `PRIMARY` | `50.0` | `100.0` |
| 4 | `500000` | `PRIMARY` | `75.0` | `150.0` |
| 5 | `1000000` | `PRIMARY` | `100.0` | `200.0` |

`upgrades.member_limit.tiers`:

| # | `cost` | `currency` | `properties.max_members` |
|---|---|---|---|
| 1 | `0` | `PRIMARY` | `4.0` |
| 2 | `50000` | `PRIMARY` | `8.0` |
| 3 | `100000` | `PRIMARY` | `12.0` |
| 4 | `200000` | `PRIMARY` | `16.0` |

`upgrades.ore_generator.tiers`:

| # | `cost` | `currency` | `properties.tier` |
|---|---|---|---|
| 1 | `25000` | `PRIMARY` | `1.0` |
| 2 | `75000` | `PRIMARY` | `2.0` |
| 3 | `250000` | `PRIMARY` | `3.0` |
| 4 | `1000000` | `PRIMARY` | `4.0` |

`upgrades.crop_growth.tiers`:

| # | `cost` | `currency` | `properties.rate_multiplier` |
|---|---|---|---|
| 1 | `20000` | `PRIMARY` | `1.10` |
| 2 | `60000` | `PRIMARY` | `1.25` |
| 3 | `150000` | `PRIMARY` | `1.50` |

`upgrades.warp_slots.tiers`:

| # | `cost` | `currency` | `properties.limit` |
|---|---|---|---|
| 1 | `25000` | `PRIMARY` | `4.0` |
| 2 | `100000` | `PRIMARY` | `6.0` |
| 3 | `300000` | `PRIMARY` | `10.0` |

`upgrades.vault_pages.tiers`:

| # | `cost` | `currency` | `properties.limit` |
|---|---|---|---|
| 1 | `40000` | `PRIMARY` | `2.0` |
| 2 | `120000` | `PRIMARY` | `4.0` |
| 3 | `400000` | `PRIMARY` | `6.0` |

`upgrades.spawner_rates.tiers`:

| # | `cost` | `currency` | `properties.rate_multiplier` |
|---|---|---|---|
| 1 | `30000` | `PRIMARY` | `1.10` |
| 2 | `90000` | `PRIMARY` | `1.25` |
| 3 | `250000` | `PRIMARY` | `1.50` |

## `upgrades.bank_limit`

How much money the island bank holds. A tier's limit is the most it holds, in money as a player writes it; give a tier no limit, or 0, and that tier holds without end. The first tier costs nothing, so it is where every island starts.

| Key | Default | What it does |
|---|---|---|
| `display-name` | `@upgrades.names.bank_limit` |  |
| `tiers` | a list of 3 entries, below |  |

`upgrades.bank_limit.tiers`:

| # | `cost` | `currency` | `properties.limit` |
|---|---|---|---|
| 1 | `0` | `PRIMARY` | `100000` |
| 2 | `50000` | `PRIMARY` | `1000000` |
| 3 | `250000` | `PRIMARY` | `10000000` |

## `upgrades.mob_drops`

More of every drop from the mobs killed on the island.

| Key | Default | What it does |
|---|---|---|
| `display-name` | `@upgrades.names.mob_drops` |  |
| `tiers` | a list of 3 entries, below |  |

`upgrades.mob_drops.tiers`:

| # | `cost` | `currency` | `properties.rate_multiplier` |
|---|---|---|---|
| 1 | `40000` | `PRIMARY` | `1.25` |
| 2 | `120000` | `PRIMARY` | `1.5` |
| 3 | `300000` | `PRIMARY` | `2.0` |

## `upgrades.island_effects`

The island's potion effects. Each property of a tier is an effect and its level, worn by the players on the island while modules/island-effects.conf names this upgrade.

| Key | Default | What it does |
|---|---|---|
| `display-name` | `@upgrades.names.island_effects` |  |
| `tiers` | a list of 3 entries, below |  |

`upgrades.island_effects.tiers`:

| # | `cost` | `currency` | `properties.haste` | `properties.speed` | `properties.jump_boost` |
|---|---|---|---|---|---|
| 1 | `50000` | `PRIMARY` | `1` |  |  |
| 2 | `150000` | `PRIMARY` | `1` | `1` |  |
| 3 | `400000` | `PRIMARY` | `2` | `1` | `1` |

## `upgrades.hopper_limit`

The limit upgrades. Each tier raises every quota of modules/limits.conf that names the upgrade by that quota's per-tier, so a tier holds no properties of its own: the limits file says how much.

| Key | Default | What it does |
|---|---|---|
| `display-name` | `@upgrades.names.hopper_limit` |  |
| `tiers` | a list of 3 entries, below |  |
| `upgrades.piston_limit.display-name` | `@upgrades.names.piston_limit` |  |
| `upgrades.piston_limit.tiers` | a list of 3 entries, below |  |
| `upgrades.redstone_limit.display-name` | `@upgrades.names.redstone_limit` |  |
| `upgrades.redstone_limit.tiers` | a list of 3 entries, below |  |
| `upgrades.brewing_limit.display-name` | `@upgrades.names.brewing_limit` |  |
| `upgrades.brewing_limit.tiers` | a list of 3 entries, below |  |
| `upgrades.spawner_limit.display-name` | `@upgrades.names.spawner_limit` |  |
| `upgrades.spawner_limit.tiers` | a list of 3 entries, below |  |
| `upgrades.villager_limit.display-name` | `@upgrades.names.villager_limit` |  |
| `upgrades.villager_limit.tiers` | a list of 3 entries, below |  |
| `upgrades.entity_limit.display-name` | `@upgrades.names.entity_limit` |  |
| `upgrades.entity_limit.tiers` | a list of 3 entries, below |  |

`upgrades.hopper_limit.tiers`:

| # | `cost` | `currency` |
|---|---|---|
| 1 | `25000` | `PRIMARY` |
| 2 | `75000` | `PRIMARY` |
| 3 | `200000` | `PRIMARY` |

`upgrades.piston_limit.tiers`:

| # | `cost` | `currency` |
|---|---|---|
| 1 | `25000` | `PRIMARY` |
| 2 | `75000` | `PRIMARY` |
| 3 | `200000` | `PRIMARY` |

`upgrades.redstone_limit.tiers`:

| # | `cost` | `currency` |
|---|---|---|
| 1 | `30000` | `PRIMARY` |
| 2 | `90000` | `PRIMARY` |
| 3 | `250000` | `PRIMARY` |

`upgrades.brewing_limit.tiers`:

| # | `cost` | `currency` |
|---|---|---|
| 1 | `20000` | `PRIMARY` |
| 2 | `60000` | `PRIMARY` |
| 3 | `180000` | `PRIMARY` |

`upgrades.spawner_limit.tiers`:

| # | `cost` | `currency` |
|---|---|---|
| 1 | `50000` | `PRIMARY` |
| 2 | `150000` | `PRIMARY` |
| 3 | `400000` | `PRIMARY` |

`upgrades.villager_limit.tiers`:

| # | `cost` | `currency` |
|---|---|---|
| 1 | `40000` | `PRIMARY` |
| 2 | `120000` | `PRIMARY` |
| 3 | `300000` | `PRIMARY` |

`upgrades.entity_limit.tiers`:

| # | `cost` | `currency` |
|---|---|---|
| 1 | `30000` | `PRIMARY` |
| 2 | `90000` | `PRIMARY` |
| 3 | `250000` | `PRIMARY` |
