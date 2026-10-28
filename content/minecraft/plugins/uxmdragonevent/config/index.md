---
title: Configuration
order: 50
description: The files, and which page documents each part of config.yml.
icon: settings
---

```
plugins/uxmDragonEvent/
├── config.yml            the event, the dragon, the world, the rules, the messages
├── rewards.yml           what players get
├── discord.yml           Discord announcements
├── locations.yml         spawn points, written by the plugin
├── autostart_data.yml    the schedule's record, written by the plugin
└── data/stats.yml        player statistics, written by the plugin
```

Change `config.yml`, `rewards.yml` and `discord.yml`, then run `/dragon reload`. A reload ends a
running event without rewards, so wait for it to finish.

Most of `config.yml` is documented with the feature it belongs to:

| Section | Page |
|---|---|
| `auto-start` | [Scheduling](../features/scheduling.md) |
| `settings` | [settings](settings.md) |
| `dragon` | [The dragon](../features/the-dragon.md) |
| `custom-world`, `world-border` | [Worlds and the border](../features/worlds.md) |
| `end-rules`, `block-blacklist`, `item-blacklist` | [End rules](../features/end-rules.md) |
| `command-blocking` | [Commands](../commands.md#commands-during-an-event) |
| `boss-bar`, `sounds`, `titles`, `messages`, `commands` | [Presentation](../features/presentation.md) |

`rewards.yml` is described in [Rewards](../features/rewards.md), and `discord.yml` in
[Discord announcements](../features/discord.md).

- [settings](settings.md)
- [Data files](data-files.md)
