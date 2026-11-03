---
title: The tool
order: 402
description: The hotbar item, its three icons, its words, when it is handed out, and the lock that keeps it in its slot.
icon: wand
---

The tool is a dye in the hotbar. A right click with it in the main hand, at the air or at a block, moves to the next
state; the click does nothing else, so it never opens a door or places the dye. The tool changes to the new state's
icon at once.

## The keys

| Key under `tool` | Default | What it does |
|---|---|---|
| `enabled` | `true` | Hand out the tool. Off, nobody gets it and the lock is off; `/hide` still works |
| `slot` | `7` | The hotbar slot, `0` to `8`. `7` is the eighth slot |
| `lock-in-slot` | `true` | The tool cannot be moved, dropped, swapped to the off hand or placed, and never drops on death |
| `give-on-join` | `true` | Hand it out when a player joins |
| `give-on-respawn` | `true` | Hand it out when a player respawns |
| `give-on-world-change` | `true` | Hand it out when a player changes world |
| `select-on-give` | `false` | Make the tool the held item when it is handed out |

The tool goes to its slot when that slot is empty or holds the tool; otherwise to the slot the player moved it to, or
the first free slot. Another item is never written over, and a player never gets two.

A player who walks into a world not in `worlds` loses the tool, and gets it back when they walk into one that is.

## The icons

| State | `material` | `glow` | `custom-model-data` |
|---|---|---|---|
| `all` | `LIME_DYE` | `false` | `0` |
| `important` | `YELLOW_DYE` | `false` | `0` |
| `none` | `GRAY_DYE` | `true` | `0` |

They are set under `tool.icons.<state>`. Every state needs an icon. A material the server does not know is drawn as
`PAPER`, with a warning in the console once. `custom-model-data` above `0` picks a resource pack model.

## The words

The tool has no name line; its tooltip is drawn from the messages files, in each player's language:

| Part | Key | Shipped, in English |
|---|---|---|
| Title | `tool.title` | `Player Visibility` |
| Category | `tool.crumb` | `Lobby` |
| Description header and text | `tool.lore.description`, `tool.description` | `Description`, `Choose who you see in the lobby. The choice waits for you when you come back.` |
| Details header | `tool.lore.details` | `Details` |
| State row | `tool.status-label`, `tool.<state>.name` | `Showing`: `Everybody`, `Staff and friends` or `Nobody` |
| Click line | `tool.<state>.lore` | `Right click to hide the crowd`, `to hide everybody`, `to show everybody` |

The words are drawn for each player on the way to them, so two players read the tool in their own language, and a
player who changes language reads it in the new one within a second.

Related: [States](states.md), [config.conf](../config/config-conf.md#tool)
