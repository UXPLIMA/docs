---
title: Configuration
order: 50
description: Every file in the plugin folder, the page that explains it, and what an update changes in them.
icon: settings
---

```
plugins/uxmSpawners/
├── config.yml            license, language, database, worlds, Auto Kill mode, debug
├── general-settings.yml  storage, breaking, friends, Auto Kill, multipliers and the rest
├── prices.yml            sell prices and the shop plugin to take them from
├── boosts.yml            boost items
├── hologram.yml          how holograms look, and their lines
├── spawners/             one file per spawner type
├── loottables/           custom drops, one file per mob
├── languages/            messages and menus, one folder per language
├── database.db           placed spawners, written by the plugin
├── logs/                 problems with single spawners, a file a day
└── backups/              copies made before an update changed your files
```

| File | Page |
|---|---|
| `config.yml` | [config.yml](config-yml.md) |
| `general-settings.yml` | [general-settings.yml](general-settings.md) |
| `spawners/` | [Spawner Types](spawner-types.md) |
| `loottables/` | [Loot Tables](loot-tables.md) |
| `prices.yml` | [Prices](prices.md) |
| `languages/` | [Language and Menus](languages.md) |
| `boosts.yml` | [Boosts](../features/boosts.md) |
| `hologram.yml` | [Holograms](../features/holograms.md) |

Every file applies with `/usp reload`, except the few settings named on [Commands](../commands.md#reload).
A file with an error is refused as a whole: the console names it, and the previous settings stay in
use.

## When a new version is installed

| Files | What the update does |
|---|---|
| `config.yml`, `general-settings.yml`, `hologram.yml`, the `messages.yml` of your language | Adds new settings with their defaults and removes settings the plugin no longer has. Values you changed are kept |
| `prices.yml`, `boosts.yml`, `spawners/`, `loottables/`, the menus | Left as they are |

Your own lists are never touched: the permission lists of drop chances, storage limits and
multipliers, and `translates`. A setting you add by hand to one of the first four files, which the
plugin does not know, is removed. A message missing from your language file shows the shipped text.

Related: [Installation](../getting-started/installation.md), [Upgrading to 5.0.0](../getting-started/upgrading.md)
