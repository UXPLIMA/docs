---
title: Installation
order: 11
description: Which of the three jars to use, the plugins it cannot start without, and the files it writes.
icon: download
---

## Requirements

| | |
|---|---|
| Server | Paper or Folia 1.21.11 or newer, 26.x.x included |
| Java | 21 or newer for 1.21.11, 25 or newer for 26.x.x |
| Required | WorldGuard and PlaceholderAPI. Without either, the plugin turns itself off |
| Required on Folia and 26.x.x | A world plugin: Worlds by TheNextLvl, or MyWorlds on Paper 26.x.x |
| Optional | Vault and an economy plugin, for the entry price |

## Which jar

The download holds three jars. Install exactly one of them:

| Jar | Server |
|---|---|
| `uxmDragonEvent-v2.6.3+Paper1.21.x.jar` | Paper 1.21.11 |
| `uxmDragonEvent-v2.6.3+Folia1.21.x.jar` | Folia 1.21.11 |
| `uxmDragonEvent-v2.6.3+MC26.x.x.jar` | Paper or Folia 26.x.x |

## World plugins

The plugin creates and deletes the event world itself. How it does that depends on what is
installed:

| Server | What the plugin uses |
|---|---|
| Paper 1.21.11 | Worlds if it is installed, otherwise MyWorlds if it is installed, otherwise the server's own world handling |
| Folia 1.21.11 | Worlds 3.x.x. Without it, the plugin will not start |
| Paper 26.x.x | Worlds 4.x.x, or MyWorlds. Without one of them, the plugin will not start |
| Folia 26.x.x | Worlds 4.x.x. Without it, the plugin will not start |

If both are installed, Worlds is used. MyWorlds needs BKCommonLib, as it always does. The console
names the one it picked at startup, on the `Loading platform support` line.

Multiverse-Core is not needed. It can stay on the server for your other worlds, but it plays no part
in the event world.

## Optional integrations

| Plugin | What it adds |
|---|---|
| Vault | Charging `event-price` to join |
| ModelEngine, ItemsAdder, BetterModel | A custom model for the dragon. See [The dragon](../features/the-dragon.md#custom-models) |
| NBTAPI | Custom NBT on the dragon |

The Discord bot is built in. Its libraries are downloaded by the server on the first start, and it
stays off until you enable it in `discord.yml`.

## First run

1. Put the jar into `plugins/` and start the server. The files are written to
   `plugins/uxmDragonEvent/`.
2. Stop the server, open `config.yml`, and set `license-key` to the key from your UXPLIMA purchase.
   Without a valid key the plugin turns itself off.
3. Check the world settings:

   ```yaml
   settings:
     lobby-world-name: world
     event-world-name: 'dragonevent_end'
     worldguard-region-name: dragonportal
   ```

   `event-world-name` must be the name of a world that does not exist yet. The plugin creates it, and
   deletes and recreates it before every event.

4. Start the server and follow [Setting up the event](setup.md).

<Callout type="warning" title="Never use an existing world as the event world">

Before every event the plugin deletes the folder of `event-world-name` and generates it again.
Pointing it at your real end, or any world players have built in, deletes that world. To fight in a
world you built, use a [custom world](custom-world.md).

</Callout>

## Files

```
plugins/uxmDragonEvent/
├── config.yml            the event, the dragon, the world, the rules, the messages
├── rewards.yml           what players get
├── discord.yml           Discord announcements
├── locations.yml         spawn points, written by the plugin
├── autostart_data.yml    the schedule's record, written by the plugin
└── data/
    ├── stats.yml         player statistics
    └── license.cache     written by the license check
```

`config.yml`, `rewards.yml` and `discord.yml` are updated by the plugin when you install a new
version: new settings are added with their defaults, and settings that no longer exist are removed.
The other files are the plugin's own. See [Data files](../config/data-files.md).
