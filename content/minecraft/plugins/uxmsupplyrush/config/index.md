---
title: Configuration
order: 50
description: Every file in the plugin folder and what belongs in it.
icon: settings
---

```
plugins/uxmSupplyRush/
├── config.yml              license, language, database, network, number format
├── general-settings.yml    tournament rules: ties, warnings, creative mode, log cleaner
├── auto-start.yml          the weekly schedule
├── events/
│   ├── example-rush.yml    a finished example to copy from
│   └── <key>.yml           one file per event: rewards and announcements
├── languages/
│   ├── en/
│   │   ├── messages.yml
│   │   └── menus/          13 menu files
│   └── tr/ es/ pt_br/ de/ fr/ ru/ pl/
├── history/                exported summaries and run logs
├── backups/                database copies taken before an upgrade (SQLite)
└── data/                   files the plugin keeps for itself
```

Most changes take effect with `/sr reload`. Changing the database or the network settings in
`config.yml` needs a restart.

- [config.yml](config-yml.md)
- [general-settings.yml](general-settings.md)
- [Event files](event-files.md)
- [Database](database.md)
- [Languages and menus](languages.md)

The weekly schedule in `auto-start.yml` is covered on [Automatic starts](../features/auto-start.md).
