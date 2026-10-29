---
title: Integrations
order: 60
description: The protection, shop, hologram, stacker and item plugins uxmSpawners works with, and what each one does.
icon: plug
---

Vault and NBTAPI are required. The plugins below are used when they are installed; nothing needs to
be set to turn them on.

## Protection

| Plugin | What it does |
|---|---|
| WorldGuard, GriefPrevention, Towny, Lands, ProtectionStones, SuperiorSkyblock2, uxmClaims | A player cannot place a spawner where the plugin would not let them build |
| Lands, SuperiorSkyblock2 | Deleting a land or disbanding an island removes the spawners in it, with their storage |
| uxmClaims | Deleting a claim lets anyone break the spawners that were in it. Two settings in `modules/other.yml` add more, see below |

Breaking and the menu do not ask the protection plugin: ownership decides. A spawner can be broken by
its owner and by staff with `uxmspawners.breakallspawners`, and opened by its owner and their friends.

With uxmClaims:

| Setting in `modules/other.yml` | Effect |
|---|---|
| `break-management.claimless-spawners-breakable-by-everyone` | Anyone can break a spawner that is not in a claim |
| `place-management.disable-placement-outside-claims` | Spawners can only be placed inside a claim |

Other protection plugins listed as soft dependencies, such as PlotSquared, FactionsUUID,
FabledSkyBlock or BlockLocker, are not checked in 4.2.0.

## Shops

ShopGUIPlus, EconomyShopGUI (free or premium) and ExcellentShop can supply sell prices. See
[Prices](config/prices.md). The money is paid through Vault, to whichever economy plugin it uses.

## Holograms

FancyHolograms 2 and 3, DecentHolograms and CMI. See [Holograms](features/holograms.md).

## Stackers and chunk loaders

| Plugin | Note |
|---|---|
| RoseStacker | Needs `trigger-death-event-for-entire-stack-kill: true` in RoseStacker's config, or a stack killed at once counts as one mob |
| WildStacker | `/uxmspawners convert` turns WildStacker spawners on SuperiorSkyblock2 islands into uxmSpawners spawners |
| WildLoaders | A chunk with an active chunk loader runs its spawners with no player nearby |

<Callout type="danger" title="RoseStacker without that setting loses drops">

It is silent for players: a spawner seems to produce a fraction of what it should, and nothing tells
them why. The console warns each time, so check it once after installing.

</Callout>

## Custom items and mobs

With ItemsAdder, a spawner type can produce an ItemsAdder item or spawn an ItemsAdder mob: write
`material: "IA-namespace:id"`. See [Spawner Types](config/spawner-types.md#itemsadder).

## Worlds

Spawners in worlds loaded and unloaded while the server runs, by Multiverse-Core, Worlds or
AdvancedSlimePaper, are loaded and unloaded with them.

Related: [Installation](getting-started/installation.md), [Breaking Rules](features/breaking.md)
