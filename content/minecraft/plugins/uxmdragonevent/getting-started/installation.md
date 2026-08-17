---
title: Installation
order: 11
description: Requirements, optional integrations, first run and the files it writes.
icon: download
---

## Requirements

| | |
|---|---|
| Server | Paper 1.20.6 or newer. Folia is supported |
| Java | 21 or newer |
| Required | An end-type world for the event |
| Recommended | Vault (entry price), PlaceholderAPI, WorldGuard (portal region) |

JDA and its dependencies are declared as libraries in `plugin.yml` and downloaded at startup — they
are only used if you enable the Discord integration.

The plugin loads with `load: POSTWORLD`, after worlds exist, which is what lets it manage the event
world.

## Optional integrations

| Plugin | What it adds |
|---|---|
| Vault | Charging `event-price` to join |
| PlaceholderAPI | The `DragonEvent` [expansion](../placeholders.md) |
| WorldGuard | The named region that holds the entry portal |
| Multiverse-Core, Worlds | Managing a custom event world |
| ModelEngine, ItemsAdder, BetterModel | A custom model for the dragon |
| NBTAPI | Custom NBT on the dragon |

## First run

1. Drop the jar into `plugins/` and start the server. The configuration is written to
   `plugins/uxmDragonEvent/`.
2. Stop the server, open `config.yml`, and set `license-key`.
3. Set the three world settings:

   ```yaml
   settings:
     lobby-world-name: world
     event-world-name: 'dragonevent_end'
     worldguard-region-name: dragonportal
   ```

4. Start the server and follow [Setting up the event](setup.md).

<Callout type="warning" title="The event world is not an ordinary world">

The plugin resets and manages `event-world-name`. Do not point it at your live end — anything built
there will not survive. Use a separate world, or a
[custom world folder](../features/worlds.md#custom-worlds).

</Callout>

## Version-specific modules

The plugin ships handlers for Paper 1.21, Folia 1.21 and newer builds, and picks the right one at
startup. There is nothing to select.

## Files

```
plugins/uxmDragonEvent/
├── config.yml            everything about the event
├── rewards.yml           what winners get
├── discord.yml           announcements
├── locations.yml         spawn points, written by /dragon save
├── autostart_data.yml    schedule state, written by the plugin
└── data/stats.yml        player statistics
```

`locations.yml`, `autostart_data.yml` and `data/stats.yml` start empty and are written by the plugin.
Do not edit them by hand while the server is running.
