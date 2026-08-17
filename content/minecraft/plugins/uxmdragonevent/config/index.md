---
title: Configuration
order: 50
description: The files, and the settings that are not covered with a feature.
icon: settings
---

```
plugins/uxmDragonEvent/
├── config.yml            the event, the dragon, the world, the rules, the messages
├── rewards.yml           what winners get
├── discord.yml           announcements
├── locations.yml         spawn points — written by /dragon save
├── autostart_data.yml    schedule state — written by the plugin
└── data/stats.yml        player statistics
```

Most of `config.yml` is documented with the feature it belongs to:

| Section | Page |
|---|---|
| `auto-start` | [Scheduling](../features/scheduling.md) |
| `dragon`, `boss-bar` | [The dragon](../features/the-dragon.md), [Presentation](../features/presentation.md) |
| `settings.join-*`, `price-*`, `end-portals` | [Joining](../features/joining.md) |
| `custom-world`, `world-border` | [Worlds](../features/worlds.md) |
| `end-rules`, `block-blacklist`, `item-blacklist` | [End rules](../features/end-rules.md) |
| `sounds`, `titles`, `messages`, `commands` | [Presentation](../features/presentation.md) |

- [settings](settings.md)
- [Data files](data-files.md)
