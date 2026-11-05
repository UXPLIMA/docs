---
title: generators.conf
order: 625
description: What the cobblestone generator makes at each tier.
icon: pickaxe
---

The file is `plugins/uxmSkyblock/modules/generators.conf`. Every default below is the shipped value.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Generators Configuration (Cobblestone / Stone Generator Ore Rates) |

## `tiers`

Each tier may write its own rates for the Nether and the End, under nether ( rates `( )` ) and end ( rates `( )` ). A tier that writes none turns up its overworld rates in every dimension.

## `tiers.0.rates`

| Key | Default | What it does |
|---|---|---|
| `COBBLESTONE` | `1.0` |  |
| `tiers.1.rates.COBBLESTONE` | `0.70` |  |
| `tiers.1.rates.COAL_ORE` | `0.20` |  |
| `tiers.1.rates.IRON_ORE` | `0.10` |  |
| `tiers.2.rates.COBBLESTONE` | `0.55` |  |
| `tiers.2.rates.COAL_ORE` | `0.20` |  |
| `tiers.2.rates.IRON_ORE` | `0.15` |  |
| `tiers.2.rates.GOLD_ORE` | `0.06` |  |
| `tiers.2.rates.LAPIS_ORE` | `0.04` |  |
| `tiers.2.nether.rates.NETHERRACK` | `0.60` |  |
| `tiers.2.nether.rates.NETHER_QUARTZ_ORE` | `0.25` |  |
| `tiers.2.nether.rates.NETHER_GOLD_ORE` | `0.15` |  |
| `tiers.2.end.rates.END_STONE` | `0.90` |  |
| `tiers.2.end.rates.OBSIDIAN` | `0.10` |  |
| `tiers.3.rates.COBBLESTONE` | `0.40` |  |
| `tiers.3.rates.IRON_ORE` | `0.20` |  |
| `tiers.3.rates.GOLD_ORE` | `0.15` |  |
| `tiers.3.rates.REDSTONE_ORE` | `0.10` |  |
| `tiers.3.rates.LAPIS_ORE` | `0.10` |  |
| `tiers.3.rates.DIAMOND_ORE` | `0.05` |  |
| `tiers.3.nether.rates.NETHERRACK` | `0.45` |  |
| `tiers.3.nether.rates.BLACKSTONE` | `0.15` |  |
| `tiers.3.nether.rates.NETHER_QUARTZ_ORE` | `0.22` |  |
| `tiers.3.nether.rates.NETHER_GOLD_ORE` | `0.15` |  |
| `tiers.3.nether.rates.ANCIENT_DEBRIS` | `0.03` |  |
| `tiers.3.end.rates.END_STONE` | `0.80` |  |
| `tiers.3.end.rates.OBSIDIAN` | `0.15` |  |
| `tiers.3.end.rates.PURPUR_BLOCK` | `0.05` |  |
| `tiers.4.rates.COBBLESTONE` | `0.30` |  |
| `tiers.4.rates.IRON_ORE` | `0.18` |  |
| `tiers.4.rates.GOLD_ORE` | `0.15` |  |
| `tiers.4.rates.REDSTONE_ORE` | `0.12` |  |
| `tiers.4.rates.DIAMOND_ORE` | `0.10` |  |
| `tiers.4.rates.EMERALD_ORE` | `0.10` |  |
| `tiers.4.rates.ANCIENT_DEBRIS` | `0.05` |  |
| `tiers.4.nether.rates.NETHERRACK` | `0.35` |  |
| `tiers.4.nether.rates.BLACKSTONE` | `0.15` |  |
| `tiers.4.nether.rates.NETHER_QUARTZ_ORE` | `0.25` |  |
| `tiers.4.nether.rates.NETHER_GOLD_ORE` | `0.17` |  |
| `tiers.4.nether.rates.ANCIENT_DEBRIS` | `0.08` |  |
| `tiers.4.end.rates.END_STONE` | `0.70` |  |
| `tiers.4.end.rates.OBSIDIAN` | `0.20` |  |
| `tiers.4.end.rates.PURPUR_BLOCK` | `0.10` |  |
