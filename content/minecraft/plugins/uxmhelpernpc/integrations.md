---
title: Integrations
order: 60
description: Region, economy and interaction hooks, and the plugins uxmHelperNPC notices on its own.
icon: plug
---

uxmHelperNPC does not implement claims, characters or currency. It asks another plugin for each,
and you pick which in `config.yml`.

## Region hooks

`hooks.region-hook` decides which plugin owns "the player's region". It answers two questions:
is the player inside their own region, and where is its centre.

| Value | Notes |
|---|---|
| `SuperiorSkyblock2` | |
| `IridiumSkyblock` | |
| `BentoBox:<world>` | Use the island world's friendly name from your addon config, e.g. `BentoBox:BSkyBlock`, `BentoBox:OneBlock` |
| `FabledSkyBlock` | |
| `Lands` | |
| `GriefPrevention` | |
| `GriefDefender` | Does not support `place-npc-on-region-creation` |
| `ProtectionStones:<world>` | Supports automatic placement, but keeping it off is recommended |
| `RClaim` | |
| `Towny` | For TownyAdvanced |
| `UltimateClaims` | |
| `uxmClaims` | |

Leaving it empty removes the region checks entirely: NPCs can then be placed anywhere outside the
disabled worlds, and automatic placement has nothing to trigger on.

## Economy hooks

`hooks.money-hook` takes `Vault` or `PlayerPoints`. It is used for the purchase price and for the
refund on return. With `settings.price: 0` you can leave it unset.

## Interaction hooks

`hooks.interaction-hook` decides what draws the NPC:

| Value | Notes |
|---|---|
| *(empty)* | The built-in packet NPC |
| `Citizens` | Per-NPC entity type supported |
| `FancyNpcs` | Per-NPC entity type supported |
| `ZNPCsPlus` | |
| `ModelSystem` | ModelEngine, BetterModel or ItemsAdder, selected in `model-settings` |

Holograms are always the plugin's own, whichever hook you choose.

## PlaceholderAPI

The plugin reads placeholders — in hologram lines, menu titles, item names and lore — but does not
publish an expansion of its own. Anything PlaceholderAPI can resolve on your server can be shown
on an NPC.

<Callout type="note" title="Everything here is a soft dependency">

Every plugin above is declared `softdepend`. A missing one is not an error at startup; it becomes
one the first time you point a hook at a plugin that is not installed. After changing a hook,
watch the console on the next `/uxmhelpernpc reload`.

</Callout>
