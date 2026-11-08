---
title: Integrations
order: 160
description: The claim, shop, stacker, permission and item plugins uxmSpawners works with, and what each one does.
icon: plug
---

Vault with an economy plugin is required. The plugins below are used when they are installed; nothing
needs to be set to turn them on, and the console names each one it picked at startup. A plugin whose
version does not fit stops uxmSpawners at startup, with a message naming it.

## Claims, islands and regions

uxmSpawners uses one of these, the first installed in this order:

| Plugin | Place | Break | Open the menu |
|---|---|---|---|
| uxmClaims | The `SPAWNER_PLACE` role | The `SPAWNER_DESTROY` role | The `CONTAINER_OPEN` role |
| SuperiorSkyblock2 | Island membership | Island membership | Island membership |
| Lands | The block place flag | The block break flag | The container flag |
| Towny | Towny's build rights | Towny's destroy rights | Towny's switch rights |
| GriefPrevention | Build trust | Build trust | Container trust |
| ProtectionStones | Region membership | Region membership | Region membership |
| WorldGuard | The build flag | The build flag | The build or chest-access flag |

Claim and island plugins come before WorldGuard, which servers often run next to one for their
spawn. The others installed are named in the console and not asked. Outside every claim, island or
region everything is allowed, except with Towny, whose own wilderness rules apply and refuse
everyone by default.

The claim's answer comes on top of ownership: an owner still needs the claim's rights, and a friend
removed from a claim loses the spawners in it. With WorldGuard and SuperiorSkyblock2, ops are let
through; other staff need the rights like anyone else. Three settings use the claim plugin:

| Setting in `general-settings.yml` | Effect |
|---|---|
| `place-management.disable-placement-outside-claims` | Spawners can only be placed inside a claim |
| `break-management.claimless-spawners-breakable-by-everyone` | Anyone can break a spawner outside every claim, such as one whose claim was deleted |
| `break-management.claim-owners-break-all-spawners` | Claim owners, and members allowed to break spawners, can break every spawner in the claim |

Changing the claim plugin needs a restart.

## Shops and economy

ShopGUIPlus, EconomyShopGUI (free or premium) and ExcellentShop can supply sell prices. See
[Prices](config/prices.md). The money is paid through Vault, to whichever economy plugin it uses.

## Stackers

| Plugin | Note |
|---|---|
| RoseStacker | Spawner mobs stack, but never with other mobs. `rosestacker.stacked-deaths` decides how a stack killed at once pays, see below |
| WildStacker | Only for `/usp convert`, which turns WildStacker spawners on SuperiorSkyblock2 islands into uxmSpawners spawners |

| `rosestacker.stacked-deaths` | A stack killed at once |
|---|---|
| `OWN_SPAWNER` | Every mob pays the spawner it came from, at that spawner's level |
| `TOP_SPAWNER` | The whole stack pays the spawner of the mob on top |
| `VANILLA` | RoseStacker drops the stack's normal loot on the ground, without levels or storage |

Keep RoseStacker's `trigger-death-event-for-entire-stack-kill` at `false`, its default. `TOP_SPAWNER`
needs it, and with `true` RoseStacker also drops a stray XP orb.

<Callout type="danger" title="Turn off spawner stacking in the stacker">

RoseStacker's `global-spawner-settings.stacking-enabled` and WildStacker's spawner stacking take over
breaking every spawner: the block goes, a plain spawner item or nothing drops, and the uxmSpawners
spawner with its levels and storage is lost. The console warns at startup while either is on. Turn
it off and let uxmSpawners stack spawners with its levels.

</Callout>

## Chunk loaders

With WildLoaders, a spawner in a chunk that has an active chunk loader runs with no player nearby.

## Permissions

With LuckPerms, storage limits and sell multipliers follow the ranks of owners who are offline: a
rank change reaches their spawners at once, and the storage of a spawner destroyed while its owner is
offline sells at their multiplier. Without it, an offline owner's spawners wait until they join.

## Holograms

Holograms need PacketEvents. See [Holograms](features/holograms.md).

## PlaceholderAPI

With PlaceholderAPI, placeholders of other plugins work in messages, menus, spawner names and
console commands of menu items. uxmSpawners adds no placeholders of its own.

## Custom items and mobs

With ItemsAdder, a spawner type can produce an ItemsAdder item or spawn an ItemsAdder mob: write
`material: "IA-namespace:id"`. See [Spawner Types](config/spawner-types.md#itemsadder).

## Worlds

Spawners in worlds loaded and unloaded while the server runs, by Multiverse-Core, Worlds or
AdvancedSlimePaper, are loaded and unloaded with them. While a world's spawners are still loading,
its spawner blocks cannot be broken.

Related: [Installation](getting-started/installation.md), [Breaking Rules](features/breaking.md)
