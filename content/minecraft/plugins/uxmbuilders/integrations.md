---
title: Integrations
order: 290
description: WorldEdit, the economy, land protection, PlaceholderAPI and Discord.
icon: plug
---

Only WorldEdit is required. Everything else is detected at startup, and a protection or economy plugin
that enables later is picked up when it loads.

| Plugin | Role | Without it |
|---|---|---|
| WorldEdit or FastAsyncWorldEdit | Reads `.schem` files; `/builders admin save` uses your selection | The plugin does not start |
| Vault or VaultUnlocked, with an economy | Prices, refunds, builder and boost purchases | Everything is free |
| WorldGuard | Region checks | |
| GriefPrevention | Claim checks, including subdivisions | |
| Towny | Town checks | |
| PlotSquared | Plot checks | |
| Lands, Residence, RedProtect, GriefDefender, BentoBox, SuperiorSkyblock2 | Through the generic `bukkitEvent` provider, off by default | |
| PlaceholderAPI | [Placeholders](placeholders.md) | No placeholders |
| LuckPerms or any permissions plugin | The numeric nodes and content permissions | Defaults only |
| Discord | [Notifications](features/notifications.md) through a webhook, no bot needed | |

## Economy

Any economy that registers with Vault or VaultUnlocked works. The provider is looked up again later if
it was missing at startup, so an economy plugin that enables after uxmBuilders still takes effect.

A refund the economy refuses is reported to the player and logged as `REFUND_FAILED` rather than shown
as paid.

## Protection

The native adapters are listed with their ids on [Land protection](features/protection.md). Another
plugin can add its own provider through the [developer API](developer.md).

## Schematics

Any `.schem` file WorldEdit can read works, up to `schematic.maxBlocks` of volume. The file is checked
for its size and dimensions before WorldEdit parses it, so an oversized or malformed upload cannot stall
the server.

Related: [Installation](getting-started/installation.md)
