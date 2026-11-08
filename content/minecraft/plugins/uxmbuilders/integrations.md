---
title: Integrations
order: 390
description: WorldEdit, the economy, land protection, PlaceholderAPI and Discord.
icon: plug
---

Nothing is required. Everything below is detected at startup, and a protection or economy plugin that
enables later is picked up when it loads.

| Plugin | Role | Without it |
|---|---|---|
| Vault or VaultUnlocked, with an economy | Prices, refunds, builder and boost purchases | Everything is free |
| WorldEdit or FastAsyncWorldEdit | A WorldEdit selection works for `/builders admin save` | The plugin's own wand selects |
| WorldGuard | Region checks | |
| GriefPrevention | Claim checks, including subdivisions | |
| Towny | Town checks | |
| PlotSquared | Plot checks | |
| uxmClaims | Claim checks, chunk by chunk | |
| Lands, Residence, RedProtect, GriefDefender, HuskClaims, BentoBox, SuperiorSkyblock2 | Through the generic `bukkitEvent` provider, off by default | |
| PlaceholderAPI | [Placeholders](placeholders.md) | No placeholders |
| LuckPerms or any permissions plugin | The numeric nodes and content permissions | Defaults only |
| Discord | [Notifications](features/notifications.md) through a webhook, no bot needed | |

## Economy

Any economy that registers with Vault or VaultUnlocked works. The provider is looked up again later if
it was missing at startup, so an economy plugin that enables after uxmBuilders still takes effect.
Restarted builds are recovered on the first tick, once every plugin has enabled, so a refund owed then
finds the economy too.

A refund the economy refuses is reported to the player and logged as `REFUND_FAILED` rather than shown
as paid.

## Protection

The native providers are listed with their ids on [Land protection](features/protection.md). Another
plugin can add its own provider through the [developer API](developer.md).

## Schematics

The plugin reads `.schem` files itself, Sponge schematics versions 1 to 3, with or without WorldEdit
installed. MCEdit `.schematic` files are not read. A file is checked for its size and dimensions before
it is parsed, against `schematic.maxBlocks` of volume, so an oversized or malformed upload cannot stall
the server.

Related: [Installation](getting-started/installation.md)
