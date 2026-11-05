---
title: fly.conf
order: 624
description: Flight on a player's own island.
icon: feather
---

The file is `plugins/uxmSkyblock/modules/fly.conf`. Every default below is the shipped value.

## `fly`

Flight on a player's own island, by /is fly.

A player flies where they stand on an island they belong to, when their island role holds the fly permission (the owner always does), they hold the permission node below and the island holds the upgrade below. Leaving that ground sets them down. Flight a player has from elsewhere, from creative or another plugin, is left alone.

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` |  |
| `required-upgrade` | empty | An upgrade of modules/upgrades.conf the island must hold a tier of before its members fly. Leave it empty and the role and the node are the only questions. |
| `permission` | `uxmskyblock.island.fly` | The permission node a player must hold as well. Leave it empty and nobody is asked for one. |
| `spare-fall` | `true` | Spare the fall of a player who was set down for leaving the island. |
