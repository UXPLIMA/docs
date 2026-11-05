---
title: settings.conf
order: 642
description: The island edge and islands with no member online.
icon: sliders-horizontal
---

What happens at the edge of an island, and on an island none of whose members is online.

The file is `plugins/uxmSkyblock/modules/settings.conf`. Every default below is the shipped value.

## `border`

The edge of an island. A player who walks off it into the space between islands is stopped at the edge when this is true. A player with uxmskyblock.admin.bypass walks on.

| Key | Default | What it does |
|---|---|---|
| `stop-crossing` | `true` |  |

## `offline-islands`

While no member of an island is online, its redstone and its spawners rest, so an island nobody plays costs the server nothing. Both come back the moment a member joins.

| Key | Default | What it does |
|---|---|---|
| `stop-redstone` | `true` | Redstone wire carries no current and pistons do not move. |
| `stop-spawners` | `true` | Spawners spawn nothing. |
