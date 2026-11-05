---
title: limits.conf
order: 631
description: The most of each costly block and creature one island holds.
icon: gauge
---

The file is `plugins/uxmSkyblock/modules/limits.conf`. Every default below is the shipped value.

## `limits`

How many of the blocks and creatures that cost the server the most one island may hold. A player who places one past the limit is told so and the block or creature is not placed.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` |  |
| `bypass-permission` | `uxmskyblock.bypass.limits` | A player with this node places past every limit. |

## `limits.quotas`

One entry per kind. The kinds are fixed: hoppers, pistons, sticky pistons, observers, droppers, dispensers, brewing stands and spawners, then villagers, armor stands, minecarts and boats.

```text
base        how many an island holds with no upgrade
upgrade-id  the upgrade in modules/upgrades.conf that raises it
per-tier    how many each tier of that upgrade adds
```

## `limits.quotas.HOPPER`

| Key | Default | What it does |
|---|---|---|
| `base` | `50` |  |
| `upgrade-id` | `hopper_limit` |  |
| `per-tier` | `50` |  |
| `limits.quotas.PISTON.base` | `32` |  |
| `limits.quotas.PISTON.upgrade-id` | `piston_limit` |  |
| `limits.quotas.PISTON.per-tier` | `32` |  |
| `limits.quotas.STICKY_PISTON.base` | `32` |  |
| `limits.quotas.STICKY_PISTON.upgrade-id` | `piston_limit` |  |
| `limits.quotas.STICKY_PISTON.per-tier` | `32` |  |
| `limits.quotas.OBSERVER.base` | `32` |  |
| `limits.quotas.OBSERVER.upgrade-id` | `redstone_limit` |  |
| `limits.quotas.OBSERVER.per-tier` | `32` |  |
| `limits.quotas.DROPPER.base` | `32` |  |
| `limits.quotas.DROPPER.upgrade-id` | `redstone_limit` |  |
| `limits.quotas.DROPPER.per-tier` | `32` |  |
| `limits.quotas.DISPENSER.base` | `32` |  |
| `limits.quotas.DISPENSER.upgrade-id` | `redstone_limit` |  |
| `limits.quotas.DISPENSER.per-tier` | `32` |  |
| `limits.quotas.BREWING_STAND.base` | `16` |  |
| `limits.quotas.BREWING_STAND.upgrade-id` | `brewing_limit` |  |
| `limits.quotas.BREWING_STAND.per-tier` | `16` |  |
| `limits.quotas.SPAWNER.base` | `25` |  |
| `limits.quotas.SPAWNER.upgrade-id` | `spawner_limit` |  |
| `limits.quotas.SPAWNER.per-tier` | `25` |  |
| `limits.quotas.VILLAGER.base` | `20` |  |
| `limits.quotas.VILLAGER.upgrade-id` | `villager_limit` |  |
| `limits.quotas.VILLAGER.per-tier` | `10` |  |
| `limits.quotas.ARMOR_STAND.base` | `30` |  |
| `limits.quotas.ARMOR_STAND.upgrade-id` | `entity_limit` |  |
| `limits.quotas.ARMOR_STAND.per-tier` | `20` |  |
| `limits.quotas.MINECART.base` | `16` |  |
| `limits.quotas.MINECART.upgrade-id` | `entity_limit` |  |
| `limits.quotas.MINECART.per-tier` | `16` |  |
| `limits.quotas.BOAT.base` | `16` |  |
| `limits.quotas.BOAT.upgrade-id` | `entity_limit` |  |
| `limits.quotas.BOAT.per-tier` | `16` |  |
