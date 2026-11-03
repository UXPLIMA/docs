---
title: Overview
order: 901
description: Economies, uxmShop, PlaceholderAPI, claim plugins, custom items and mobs, and Bedrock.
icon: puzzle
---

None of these is required. Each is read only when it is installed.

| Plugin | What uxmMinions does with it |
|---|---|
| Vault, VaultUnlocked, PlayerPoints, EcoBits, Treasury | `economy.currency`: sales, level costs, store prices, teleports |
| uxmShop | The `uxmshop` price source for the seller |
| PlaceholderAPI | Publishes the [placeholders](../placeholders/reference.md); `placeholder` requirements |
| WorldGuard, GriefPrevention, Lands, Towny, Factions, Residence, PlotSquared, HuskClaims, HuskTowns, Kingdoms, BentoBox, SuperiorSkyblock2, GriefDefender, SimpleClaimSystem, ExcellentClaims, Homestead, RClaim, XClaim, uxmClaims | Islands as limit scopes, building rights, working inside the claim, losing the land |
| Oraxen, Nexo, ItemsAdder, CraftEngine | Their items in item lists |
| MythicMobs, LevelledMobs, RoseStacker | Their mobs in a slayer's lists |
| CustomCrops, CustomFishing, PyroFishingPro, InfiniteFishing | Their crops and catches |
| mcMMO | Skill levels in requirements |
| HeadDatabase ids | `hdb:<id>` heads in `skins.conf` |
| Floodgate, Geyser | Bedrock players get the windows as native forms |
| JetsMinions, LitMinions | Read once by `/uxmminions migrate` |
| uxmLang | One language choice for every UXM plugin |

No hologram plugin is needed: the hologram is drawn through uxmLib.

Related: [Installation](../getting-started/installation.md)
