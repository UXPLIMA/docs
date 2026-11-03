---
title: The hotbar tool
order: 304
description: The item that opens the window, its slot, locking, when it is given and in which worlds.
icon: compass
---

The `tool` block of [config.conf](../config/config-conf.md#tool) hands out an item that opens the window on a right
click.

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `true` | Whether the item is handed out |
| `material` | `"COMPASS"` | Its material |
| `slot` | `4` | A hotbar slot, 0 to 8. An item already there stays, and the tool takes the first free slot until this one is free |
| `locked` | `true` | Whether it may be moved, dropped or swapped out of its slot |
| `give-on-join` | `true` | Handed out at join |
| `give-on-world-change` | `true` | Checked again on a world change |
| `permission` | `""` | The node a player needs to be given it; empty means everybody |
| `worlds.mode` | `"everywhere"` | `everywhere`, `only` (only the worlds in `list`) or `except` (every world but those) |
| `worlds.list` | `[]` | World names, matched without case |

Its name and lore are `tool.name` and `tool.lore` in the messages files: "Lobbies", "Right click to choose a lobby".

A server that hands out its hotbar with uxmJoinItems can turn this off and give an item that runs `/lobby`.

Related: [config.conf](../config/config-conf.md#tool)
