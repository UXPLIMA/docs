---
title: Overview
order: 901
description: PlaceholderAPI, economies, nineteen claim plugins, custom items, other enchantment plugins, skills and Bedrock.
icon: puzzle
---

None of these is required. Each is read only when it is installed.

| Plugin | What uxmEnchants does with it |
|---|---|
| PlaceholderAPI | Registers the [placeholders](../placeholders/reference.md); resolves placeholders in conditions |
| Vault | `money >= ...` conditions and `[take-money]` actions |
| WorldGuard, GriefPrevention, Lands, Towny, Factions, Residence, PlotSquared, HuskClaims, HuskTowns, Kingdoms, BentoBox, SuperiorSkyblock2, GriefDefender, SimpleClaimSystem, ExcellentClaims, Homestead, RClaim, XClaim, uxmClaims | Block effects skip blocks the player may not break; see [Protection](../features/protection.md#claims) |
| Oraxen, Nexo, ItemsAdder, CraftEngine | Their items in `targets`, `anvil-items`, `table-items` and material filters: `"oraxen:ruby_sword"` |
| MythicMobs, LevelledMobs, RoseStacker, CustomCrops, CustomFishing, PyroFishingPro, InfiniteFishing | Their mobs and harvests named the same way in content files |
| EcoEnchants | A `conflicts` entry may name one of its enchantments; the anvil and the table read them off the item |
| mcMMO | `%mcmmo_level_<skill>%` in conditions, without PlaceholderAPI |
| Floodgate, Geyser | Bedrock players get the windows as native forms |
| uxmLang | One language choice for every UXM plugin |

The doctor's `other enchantments` and `claims` lines say whether one answered.

Related: [Installation](../getting-started/installation.md)
