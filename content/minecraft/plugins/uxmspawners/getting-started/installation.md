---
title: Installation
order: 112
description: Requirements, the first start, the files and the settings to check before players place spawners.
icon: download
---

## Requirements

| Requirement | Detail |
|---|---|
| Server | Paper 1.21.11 or newer, or a fork of Paper. Folia is supported |
| Java | 21 or newer |
| Vault | Required, with an economy plugin behind it |
| LuckPerms | Required. Storage limits, sell multipliers and spawn conditions read owners' ranks through it, also while they are offline |
| PacketEvents | Optional. Holograms need it, see [Holograms](../features/holograms.md) |
| License | A uxmSpawners license key from UXPLIMA |
| Internet on the first start | The plugin downloads its libraries from Maven Central and checks the license with UXPLIMA |

Without Vault and LuckPerms the server does not load the plugin at all. Without an economy plugin it
stops at startup and says so. NBTAPI and a hologram plugin are no longer needed. Everything else is
optional; see [Integrations](../integrations.md).

When uxmSpawners cannot start, it stops the whole server and the console says why, so its spawners
are never left breakable as plain vanilla spawners. `stop-server-on-failure: false` in `config.yml`
keeps the server running instead. See [config.yml](../config/config-yml.md#a-failed-start).

## First start

1. Put the jar into `plugins/`, next to Vault and LuckPerms, and start the server. It writes its
   files, then stops the server, because no license key is set yet.
2. Set `license-key` in `config.yml`. If you do not have one, open a ticket in the UXPLIMA Discord.
3. Set `config.lang` to `en`, `tr`, `pl`, `de` or `fr`.
4. Start the server again.

```
plugins/uxmSpawners/
├── config.yml            license, language, database, failed starts, worlds, Auto Kill mode, debug
├── general-settings.yml  storage, breaking, friends, Auto Kill, multipliers and the rest
├── prices.yml            sell prices and the shop plugin to take them from
├── boosts.yml            boost items
├── hologram.yml          how holograms look, and their lines
├── spawners/             one file per spawner type: default.yml, and diamond.yml as an example
├── loottables/           custom drops per mob, with an example
├── languages/            one folder per language: messages.yml and menus/
├── database.db           every placed spawner, written by the plugin
├── item-signing-key.txt  a copy of the key that signs spawner items; keep it private
├── data/                 files the plugin keeps for itself
├── logs/                 a file a day about problems with single spawners
└── backups/              copies the plugin made before updating your files
```

<Callout type="danger" title="One database per server">

uxmSpawners keeps the spawners in memory and writes them to its database. Two servers on one MySQL
database overwrite and delete each other's spawners. Give every server a database, or a schema, of
its own.

</Callout>

## What to check before opening

| Setting | File | Why |
|---|---|---|
| `world-management` | `config.yml` | Worlds where nobody can place or break a spawner, vanilla ones included. The default lists `testWorld`. Peaceful worlds count as disabled unless `work-in-peaceful` is on |
| `use-modern-auto-kill` | `config.yml` | Leave it on: Auto Kill then makes the loot directly instead of spawning and killing mobs |
| `default-prices`, `price-provider` | `prices.yml` | What a spawner's output sells for. See [Prices](../config/prices.md) |
| `break-management.mode` | `general-settings.yml` | Whether a tool is needed to break spawners. See [Breaking Rules](../features/breaking.md) |
| `spawn-conditions` | `spawners/default.yml` | When spawners work. A player within 16 blocks by default. See [Spawn Conditions](../features/spawn-conditions.md) |
| `vanilla-spawner-support.on-break` | `general-settings.yml` | What a natural spawner drops when broken. A uxmSpawners spawner by default |
| `break-with-storage` | `general-settings.yml` | Whether a spawner with something in storage can be broken. Not by default |
| `storage-limit` | `general-settings.yml` | How much one spawner holds |
| The player permissions | Your permission plugin | Auto Kill, the storage switches and friends are for ops until you grant them. See [Permissions](../permissions.md) |

A claim, island or region plugin is found on its own: the plugin uses the first one installed and
names it in the console. See [Integrations](../integrations.md#claims-islands-and-regions).

## Testing

Give yourself a spawner with `/usp give <you> zombie`, place it, and right-click it: the menu opens.
Turn Auto Kill on from the menu and stay within 16 blocks: the storage fills every 8 seconds.

Changes to the files apply with `/usp reload`.

Related: [Basic Concepts](concepts.md), [config.yml](../config/config-yml.md)
