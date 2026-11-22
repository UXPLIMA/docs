---
title: Integrations
order: 170
description: Every plugin uxmClaims works with, and the module that handles it.
icon: plug-zap
---

Every integration is optional and lives in a module. uxmClaims starts with none of these plugins
installed; a module whose plugin is missing does nothing or stays off with the reason in
`/uxm modules`. Each module's page has its settings and limits.

## Plugins

| Plugin | Adds | Module |
|---|---|---|
| Vault and an economy plugin | Prices, refunds, banks, taxes, plot sales and rents | [Economy](modules/economy.md) |
| LuckPerms | Limits, abilities and price nodes read through LuckPerms (offline players loaded on demand), LuckPerms contexts such as `uxmclaims:in-land` | [LuckPerms](modules/luckperms.md) |
| WorldGuard | Keeps claims out of regions, the `uxmclaims-claim` region flag | [WorldGuard](modules/worldguard.md) |
| PlaceholderAPI | The `%uxmclaims_...%` placeholders, see [Placeholders](placeholders.md) | [PlaceholderAPI](modules/placeholders.md) |
| Dynmap, BlueMap, Pl3xMap, squaremap | Lands on every installed web map at once, layered by owner | [Web Maps](modules/webmap.md) |
| Geyser or Floodgate | Bedrock forms for menus, input and confirmations | [Bedrock](modules/bedrock.md) |
| DiscordSRV | Town, nation and ally chat to Discord channels and back, event notices | [Discord](modules/discord.md) |
| CoreProtect | Undoing block changes in a land's chunks when it is unclaimed | [Rollback](modules/rollback.md) |
| Skript 2.12 or newer | Events, conditions, effects and expressions for lands, towns and nations | [Skript](modules/skript.md) (add-on) |
| Towny, Lands, GriefPrevention, HuskClaims, HuskTowns | A one-time import of their data | [Importers](modules/importers.md) |

## Without a plugin

| Feature | Plugin-free option |
|---|---|
| Discord | The [Discord](modules/discord.md) module can run its own bot (`backend: bot`), and [Webhooks](modules/webhooks.md) posts embeds with a webhook URL alone |
| Java dialogs | [Dialogs](modules/dialogs.md) uses the server's own dialog screens on versions that have them, and falls back to chests and chat elsewhere |
| Rollback | [Rollback](modules/rollback.md) keeps its own snapshots; CoreProtect is one of its strategies, not a requirement |

## What happens when one is missing

| Missing | Result |
|---|---|
| Vault | The economy module stays off (`needs the plugin Vault`) and every price is free; modules that require it, such as Bank, stay off too |
| An economy plugin behind Vault | The economy module runs; with `when-no-provider: FREE` (the default) paid actions are free and one warning is logged, with `REFUSE` they are refused until an economy registers with Vault |
| LuckPerms | Limits come from the online player's permissions through the server; offline players get the defaults |
| WorldGuard | Regions are not checked |
| PlaceholderAPI | Other plugins cannot read `%uxmclaims_...%` |
| A map plugin | Nothing is drawn |
| Floodgate and Geyser-Spigot | The Bedrock module stays off; Bedrock players joining through a proxy get the Java chest menus |
| DiscordSRV | The Discord module with `backend: discordsrv` cannot bridge; switch to `backend: bot` |
| CoreProtect | `/uxm rollback coreprotect` is not available; the `coreprotect` unclaim strategy is described on [Rollback](modules/rollback.md) (on Folia it always uses snapshots) |

## Mirror events for other plugins

uxmClaims has its own event bus. For Skript and plain Bukkit listeners it also fires each event as a
Bukkit event, controlled by `api.bukkit-events` in `config.yml`:

| Key | Default | What it does |
|---|---|---|
| `api.bukkit-events.enabled` | `true` | Mirror uxmClaims events as Bukkit events |
| `api.bukkit-events.pre-events` | `true` | Also mirror the cancellable `Pre` events |
| `api.bukkit-events.movement` | `true` | Fire `LandEnterEvent` and `LandLeaveEvent` on border crossings |

Turning these off stops the matching Skript events too. Listening from Java is on
[Events](developer/events.md).

Related: [Modules](modules/index.md), [Placeholders](placeholders.md), [Developer](developer/index.md)
