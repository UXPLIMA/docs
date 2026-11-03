---
title: Operator commands
order: 203
description: Every /uxmcosmetics subcommand, its permission, its arguments and what it answers.
icon: shield
---

`/uxmcosmetics`, aliases `/uxmcos` and `/cosmeticsadmin`. Every node is `op` by default.

## Wardrobes

| Command | Permission | What it does |
|---|---|---|
| `/uxmcosmetics grant <player> <cosmetic>` | `uxmcosmetics.admin.grant` | Gives a player a piece, online or away. `admin.granted`, or `admin.grant-already` |
| `/uxmcosmetics revoke <player> <cosmetic>` | `uxmcosmetics.admin.revoke` | Takes it away and off. `admin.revoked`, or `admin.revoke-not-owned` |

A grant writes ownership; a node-only piece cannot be revoked here, only by the permission plugin. A player the server
has never seen answers `error.unknown-player`.

## Box stations

| Command | Permission | What it does |
|---|---|---|
| `/uxmcosmetics station add <box\|all> [block]` | `uxmcosmetics.admin.station` | Binds the block you look at, or the block written, to one box or to every box |
| `/uxmcosmetics station set <station> <piece> <value>` | `uxmcosmetics.admin.station` | Changes one piece of one station |
| `/uxmcosmetics station show <station>` | `uxmcosmetics.admin.station` | Prints every piece of one station |
| `/uxmcosmetics station remove <station>` | `uxmcosmetics.admin.station` | Takes the station away; the block stays |
| `/uxmcosmetics station list` | `uxmcosmetics.admin.station` | Every station: id, box, world and block |
| `/uxmcosmetics station` | `uxmcosmetics.admin.station` | The same list |

`[block]` is left out or `here` for the block you look at (within reach), or `<x> <y> <z> [world]`. From the console
the world is required. The answer names the new station's id.

### station set pieces

| Piece | Value |
|---|---|
| `box` | A box id, or `all` |
| `position` | `<x> <y> <z> [world]`, or `here` |
| `radius` | Blocks a click may be from the block; above zero |
| `particle` | A particle name |
| `pattern` | `HALO`, `ORBIT`, `HELIX`, `TRAIL`, `CLOAK` or `WINGS` |
| `effect-radius` | How wide the figure is; above zero |
| `effect-height` | How far above the block it floats |
| `period` | Milliseconds between two frames, such as `150` or `150ms` |
| `sound` | A sound key, or `none` |
| `use-particle` | A particle burst on use, or `none` |
| `hologram` | The lines, separated by `\|`, or `none` |
| `hologram-height` | How far above the block the sign floats |

The change is written to `stations.conf` at once.

## The plugin

| Command | Permission | What it does |
|---|---|---|
| `/uxmcosmetics` | `uxmcosmetics.admin.version` | Says to add `help` |
| `/uxmcosmetics reload` | `uxmcosmetics.admin.reload` | Reads the configuration, the families, the boxes, the stations, the windows and the messages again |
| `/uxmcosmetics cleanup` | `uxmcosmetics.admin.cleanup` | Removes every cosmetic entity whose owner is not online, and says how many |
| `/uxmcosmetics doctor` | `uxmcosmetics.admin.doctor` | The health checks, one line each |
| `/uxmcosmetics version` | `uxmcosmetics.admin.version` | The version and how many cosmetics are loaded |

## Reload

A reload swaps the configuration and the content in one go and takes the station signs down and puts them up again.
A file that does not parse stops the reload with `admin.reload-failed`, and the server keeps what worked. `storage`,
`animation`, `permissions.wildcard` and `commands.conf` need a restart.

Related: [Overview](overview.md), [Box stations](../features/stations.md), [Permissions](../permissions/reference.md)
