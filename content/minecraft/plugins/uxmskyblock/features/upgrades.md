---
title: Upgrades
order: 404
description: The seventeen upgrades and how a tier is bought.
icon: arrow-up-circle
---

An upgrade has tiers, bought in order from the [upgrades window](../menus/economy.md#upgrades) with money from the bank.
Buying needs `uxmskyblock.island.upgrade` and the island permission Take money from the bank; Spawner Rates also asks
Upgrade spawners. A purchase and its charge happen together or not at all.

| Upgrade | What a tier does |
|---|---|
| Island Size | Pushes the border out, so the island has room to grow |
| Team Size | Lets more players belong to the island |
| Ore Generator | Makes the cobblestone generator give richer ore; see [generators.conf](../config/generators-conf.md) |
| Crop Growth | Makes the island's crops grow faster |
| Warp Slots | Lets the island open more warps |
| Vault Pages | Adds a page to the shared vault |
| Spawner Rates | Makes the island's spawners run faster |
| Bank Limit | Lets the bank hold more money; the first tier costs nothing |
| Mob Drops | More of every drop from mobs killed on the island |
| Island Effects | Potion effects members wear on the island; see [island-effects.conf](../config/island-effects-conf.md) |
| Hopper Limit | More hoppers |
| Piston Limit | More pistons and sticky pistons |
| Redstone Limit | More observers, droppers and dispensers |
| Brewing Limit | More brewing stands |
| Spawner Limit | More spawners |
| Villager Limit | More villagers |
| Entity Limit | More armour stands, minecarts and boats |

Every tier's price and what it sets are in [upgrades.conf](../config/upgrades-conf.md). A tier's `cost` is written in
hundredths of the island's money: `cost = 50000` is 500.00 in the window. The limits themselves are
in [limits.conf](../config/limits-conf.md).
