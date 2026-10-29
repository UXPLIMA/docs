---
title: Installation
order: 12
description: Requirements, the first start, the files and the settings to check before players place spawners.
icon: download
---

## Requirements

| Requirement | Detail |
|---|---|
| Server | Paper 1.21.4 to 1.21.11, or a fork of Paper |
| Java | 21 or newer |
| Vault | Required, with an economy plugin and a permission plugin such as LuckPerms behind it |
| NBTAPI | Required. Spawner, boost and pickaxe items are told apart by data stored on the item |
| License | A uxmSpawners license key from UXPLIMA |

Without Vault, an economy plugin or NBTAPI, the plugin turns itself off at startup and says which one
is missing. Without a permission plugin registered in Vault it fails to start with an error.

Everything else is optional. See [Integrations](../integrations.md).

## First start

1. Put the jar into `plugins/`, next to Vault and NBTAPI, and start the server. It writes
   `config.yml` and stops, because no license key is set yet.
2. Set `license-key` in `config.yml`. If you do not have one, open a ticket in the UXPLIMA Discord.
3. Set `config.lang` to `en`, `tr`, `pl`, `de` or `fr`.
4. Start the server again. The rest of the files are written now.

```
plugins/uxmSpawners/
├── config.yml          license, language, worlds, spawner types, prices
├── modules/            one file per feature
├── lang/               lang_en.yml and the others: messages, menus, mob names
├── loottables/         custom drops per mob, with an example
├── database.db         every placed spawner, written by the plugin
└── logs/errors.txt     problems the plugin wrote down, such as a missing price
```

<Callout type="danger" title="RoseStacker needs one setting changed">

With RoseStacker installed, set `trigger-death-event-for-entire-stack-kill: true` in RoseStacker's
config. Otherwise a stack killed at once counts as one mob, and spawner storage quietly receives a
fraction of what it should. The console warns about it each time it happens.

</Callout>

## What to check before opening

| Setting | File | Why |
|---|---|---|
| `disabled-worlds` | `config.yml` | Nobody can place or break a spawner there, vanilla ones included. The default lists `testWorld` |
| `use-modern-autokill` | `config.yml` | Leave it on: auto-killed spawners make their loot directly instead of spawning and killing mobs |
| `itemPrices` | `config.yml` | What a spawner's output sells for. See [Prices](../config/prices.md) |
| `break-management.mode` | `modules/other.yml` | Whether a tool is needed to break spawners. See [Breaking Rules](../features/breaking.md) |
| `vanilla-spawner-support` | `modules/other.yml` | What happens to spawners that were already in the world |
| `storage-limit` | `modules/storage_limits.yml` | How much one spawner holds |
| `auto-save.interval` | `config.yml` | Seconds between saves of what spawners hold. 300 is the default |

## Testing

Give yourself a spawner with `/uxmspawners give <you> zombie`, place it, and right-click it: the
menu opens. Turn auto-kill on from the menu, which needs `uxmspawners.autokill`, and stay within 16
blocks: the storage fills every 8 seconds.

Changes to the files apply with `/uxmspawners reload`.

Related: [Basic Concepts](concepts.md), [config.yml](../config/config-yml.md)
