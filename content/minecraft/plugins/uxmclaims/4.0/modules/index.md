---
title: Modules
order: 130
description: Every feature is a module you turn on or off. One page each.
icon: blocks
---

uxmClaims 4 is a kernel and the modules below, all in one jar. Each module owns its commands,
permission nodes, placeholders, menus, tables and its folder at `plugins/uxmClaims/modules/<id>/`. A
module that is off registers nothing and holds no state, and its data stays where it is.

Turn a module on or off in `modules.yml` and restart; `/uxm modules` shows each module's state and,
for one that did not start, the reason. See [modules.yml](../configuration/modules-yml.md).

The Default column is a clean install with no preset applied. A preset changes it: see
[Presets](../configuration/presets.md). Requires lists the modules that must run too; a module whose
requirement is off stays off.

## Land and protection

| Module | What it does | Default | Requires |
|---|---|---|---|
| [Lands](lands.md) | Lands made of chunks, roles, members, flags, world and wilderness rules, `/claim` | on | none |
| [Areas](areas.md) | Block-precise boxes inside a land with their own roles, members and flags | off | lands |
| [Expiry](expiry.md) | An end date per land, paid extensions, reminders, deletion when it runs out | on | lands |
| [Invites](invites.md) | Sending, accepting and declining land invites | on | lands |
| [Bans](bans.md) | Banning players from a land | on | lands |
| [Warps](warps.md) | Named teleport points inside a land, public warp list | on | lands |
| [Land Vault](vault.md) | A shared chest per land | on | lands |
| [Claim Block](claim-block.md) | The physical block placed at a new land's spawn | on | lands |
| [Holograms](holograms.md) | Text displays above claim blocks, and a hologram service for other modules | on | lands |
| [Borders](borders.md) | Particle walls along land borders | on | lands |
| [Notifications](notifications.md) | Titles when a player enters or leaves a land | on | lands |
| [Land Chat](land-chat.md) | A chat channel per land | on | lands |
| [Limits](limits.md) | Limits and delays from permission nodes | on | none |
| [Economy](economy.md) | Money through Vault, prices and refunds | on | none |
| [Claim Points](claim-points.md) | Chunk allowance earned by playing | off | lands |
| [Combat](combat.md) | Combat tag: no teleports, flight or listed commands while tagged | off | none |
| [Newbie Protection](newbie-protection.md) | No PvP damage for new players for a while | off | none |

## Towns, nations and the economy

| Module | What it does | Default | Requires |
|---|---|---|---|
| [Towns](towns.md) | Towns with a mayor, residents, ranks, a home land and outposts | off | lands |
| [Nations](nations.md) | Groups of towns with a capital, ranks and a spawn | off | lands, towns |
| [Relations](relations.md) | Ally, enemy, truce and neutral between lands, towns and nations | off | lands |
| [Bank](bank.md) | A treasury per town, nation and, optionally, land | off | economy, lands |
| [Taxes](taxes.md) | The daily cycle: taxes, upkeep, debts, bankruptcy | off | bank, lands |
| [Inbox](inbox.md) | Notifications kept for offline players | off | lands |
| [Plots](plots.md) | Areas for sale or rent, plot types, signs and a market | off | areas |
| [Levels](levels.md) | Town and nation levels with requirements and rewards | off | towns |

## Conflict and land life cycle

| Module | What it does | Default | Requires |
|---|---|---|---|
| [War](war.md) | Declared wars: sieges and capture wars, goals, truces | off | relations |
| [Jails](jails.md) | Town jails, sentences, bail and outlaws | off | towns |
| [Ruins](ruins.md) | Inactive lands and towns fall into ruin and can be reclaimed | off | lands |
| [Rollback](rollback.md) | Snapshots of a land's chunks, restore on unclaim | off | lands |
| [Random Teleport](rtp.md) | Random teleport into the wilderness | off | lands |
| [Productive Land](productive-land.md) | Crop speed, spawner speed and mob caps per land | off | lands |
| [Leaderboards](leaderboards.md) | Rankings of lands, towns and nations, menus and board holograms | off | lands |
| [Group Chat](group-chat.md) | Town, nation and ally chat channels | off | towns |

## Integrations and tools

| Module | What it does | Default | Requires |
|---|---|---|---|
| [PlaceholderAPI](placeholders.md) | The `%uxmclaims_...%` placeholders about lands and players | on | none |
| [Web Maps](webmap.md) | Lands on Dynmap, BlueMap, Pl3xMap and squaremap | on | none |
| [Webhooks](webhooks.md) | Discord embeds for land, town and nation events | on | none |
| [WorldGuard](worldguard.md) | Keeps claims out of regions, adds a region flag | on | none |
| [LuckPerms](luckperms.md) | Offline limits and LuckPerms contexts | on | lands |
| [Aliases](aliases.md) | Your own command aliases, Towny and Lands command profiles | on | none |
| [Dialogs](dialogs.md) | Java dialog screens for menus and text input | on | none |
| [Bedrock](bedrock.md) | Bedrock forms through Geyser or Floodgate | on | none |
| [Discord](discord.md) | Chat and notices to Discord, through DiscordSRV or its own bot | off | none |
| [Network](network.md) | Several servers on one database | off | none |
| [Importers](importers.md) | Imports from Towny, Lands, GriefPrevention, HuskClaims and HuskTowns | off | lands |
| [Audit Log](audit-log.md) | A log of every change, `/uxm audit` | off | none |
| [Automation](automation.md) | Rules that run commands and messages on events | off | none |
| [Legacy API](legacy-api.md) | Keeps add-ons built for the 3.x API working | on | lands |
| [Skript](skript.md) | Skript syntax for lands, towns and nations (add-on) | add-on | none |

An integration module whose plugin is not installed starts and does nothing, or stays off with the
reason in `/uxm modules`; its page says which.

Related: [modules.yml](../configuration/modules-yml.md), [Presets](../configuration/presets.md), [Permissions](../permissions.md)
