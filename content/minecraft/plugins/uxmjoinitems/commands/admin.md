---
title: Admin commands
order: 202
description: Giving, giving all, clearing, reload, version, doctor and another player's language.
icon: wrench
---

| Command | Node | What it does |
|---|---|---|
| `/uxmjoinitems` | `uxmjoinitems.admin.version` | The version |
| `/uxmjoinitems give <player> <item>` | `uxmjoinitems.admin.give` | Gives one item, through the same checks as a trigger |
| `/uxmjoinitems giveall <player>` | `uxmjoinitems.admin.giveall` | Gives a player every item they may hold, whatever its triggers |
| `/uxmjoinitems clear <player>` | `uxmjoinitems.admin.clear` | Takes every item this plugin handed out off a player, wherever it sits |
| `/uxmjoinitems reload` | `uxmjoinitems.admin.reload` | Reads the configuration, the items and the events again, and says how many items |
| `/uxmjoinitems version` | `uxmjoinitems.admin.version` | The version this server runs |
| `/uxmjoinitems doctor` | `uxmjoinitems.admin.doctor` | The `content` check |
| `/uxmjoinitems lang set-player <player> <language>` | `uxmjoinitems.admin.language` | Sets the language another player reads |
| `/uxmjoinitems lang reset-player <player>` | `uxmjoinitems.admin.language` | Clears it |

`give` answers why when an item does not land:

| Reply | Why |
|---|---|
| `unknown-item` | No file under `items/` has that id |
| `target-offline` | The player is not on this server |
| `outside-region` | The player is outside every region the item allows |
| `guarded` | A `skip-when` guard says the player is busy |
| `no-room` | No free slot |

`give`, `giveall` and `clear` take a player on this server. An item with no triggers is handed out only by `give` and
`giveall`.

Related: [Concepts](../getting-started/concepts.md#before-an-item-is-placed)
