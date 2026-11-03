---
title: Overview
order: 801
description: Economies, PlaceholderAPI, claim plugins, custom items and mobs, stackers, skills, pets and Bedrock.
icon: puzzle
---

None of these is required. Each is read only when it is installed.

| Plugin | What uxmJobs does with it |
|---|---|
| Vault, VaultUnlocked | Money wages and costs |
| PlayerPoints, EcoBits, Treasury | Cost backends: `playerpoints`, `ecobits:<currency>`, `treasury` |
| PlaceholderAPI | Publishes the [placeholders](../placeholders/reference.md); any `%placeholder%` in an expression; the `placeholder` action type and backend |
| uxmClaims, Lands, GriefPrevention, GriefDefender, ExcellentClaims, SimpleClaimSystem, RClaim, XClaim, Homestead, WorldGuard, Towny, Kingdoms, HuskClaims, HuskTowns, Factions, BentoBox, Residence, PlotSquared, SuperiorSkyblock2 | Work in another player's claim pays `foreign-claim-multiplier`; see [Protections](../features/protections.md#claims) |
| Oraxen, Nexo, ItemsAdder, CraftEngine | Their items as targets in a price table |
| MythicMobs, LevelledMobs | Their mobs as targets |
| RoseStacker | A stacked kill pays for every creature |
| CustomCrops, CustomFishing, PyroFishingPro, InfiniteFishing | Their harvests and catches as targets |
| mcMMO | Skill levels in requirements |
| MyPet | Damage a pet deals counts for its owner |
| Floodgate, Geyser | Bedrock players get the windows as native forms |
| uxmLang | One language choice for every UXM plugin |

Any other economy can be reached through the placeholder backend or a bridge; see [Economy](../features/economy.md).

Related: [Installation](../getting-started/installation.md)
