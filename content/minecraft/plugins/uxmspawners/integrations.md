---
title: Integrations
order: 60
description: Shop, stacker and protection plugins, and the settings each one needs.
icon: plug
---

Vault and NBTAPI are required. Everything else is optional and hooked when present.

## Protection and land

Spawner placing, breaking and menu access respect these:

| Plugin | |
|---|---|
| uxmClaims | Also enables `claimless-spawners-breakable-by-everyone` |
| WorldGuard | |
| SuperiorSkyblock2 | |
| FabledSkyBlock | |
| Lands | |
| Towny | |
| FactionsUUID | |
| GriefPrevention | |
| ProtectionStones | |
| PlotSquared | |
| BlockLocker | |

## Shops

ShopGUIPlus, EconomyShopGUI (free and premium) and ExcellentShop can all supply sell prices: see
[Prices](config/prices.md).

## Economy

Vault is required. LiteEconomy, CMI and SuperMobCoins are recognised alongside it.

## Holograms

DecentHolograms, FancyHolograms v2 and v3, and CMI. See [Holograms](features/holograms.md).

## Stacking and chunk loading

| Plugin | Note |
|---|---|
| RoseStacker | **Requires** `trigger-death-event-for-entire-stack-kill` enabled in RoseStacker |
| WildStacker | Existing spawners can be brought over with `/uxmspawners convert` |
| WildLoaders | Chunk loaders are recognised so spawners keep running |

## Items and worlds

ItemsAdder supplies custom item textures for spawner and boost items. Multiverse-Core and Worlds
are recognised for world resolution.

<Callout type="danger" title="RoseStacker without that setting loses drops">

It is worth repeating, because it is silent: with RoseStacker installed and
`trigger-death-event-for-entire-stack-kill` disabled, only one mob of a stack is counted. Players
see a spawner producing a fraction of what it should and no error anywhere.

</Callout>
