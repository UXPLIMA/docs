---
title: Configuration
order: 50
description: Every file in the plugin folder and the page that explains it.
icon: settings
---

```
plugins/uxmSpawners/
├── config.yml          license, language, worlds, saving, spawner types, prices
├── modules/            one file per feature
├── lang/               messages, menus and mob names, one file per language
├── loottables/         custom drops, one file per mob
├── database.db         placed spawners, written by the plugin
└── logs/errors.txt     problems the plugin noted
```

| File or section | Page |
|---|---|
| `config.yml` | [config.yml](config-yml.md) |
| `config.yml`: `spawners` | [Spawner Types](spawner-types.md) |
| `config.yml`: `itemPrices` | [Prices](prices.md) |
| `modules/` | [Modules](modules.md) |
| `loottables/` | [Loot Tables](loot-tables.md) |
| `lang/` | [Language and Menus](languages.md) |

When a new version is installed, the plugin adds new settings to your files and keeps what you
changed. Your spawner types, prices and permission lists are never touched. Copies of the old files
are kept in `backups/`.

- [config.yml](config-yml.md)
- [Spawner Types](spawner-types.md)
- [Modules](modules.md)
- [Loot Tables](loot-tables.md)
- [Prices](prices.md)
- [Language and Menus](languages.md)
