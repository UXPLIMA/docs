---
title: Overview
order: 501
description: The fourteen window files, who opens them, refresh blocks, view gates and Bedrock.
icon: layout-template
---

| Window | File | Opened by | Rows |
|---|---|---|---|
| [Panel](panel.md) | `menus/panel.conf` | A click on a minion | 5 |
| [Your minions](my-minions.md) | `menus/my-minions.conf` | `/minions` | 6 |
| [Pocket](pocket.md) | `menus/pocket.conf` | The ender chest in your minions | 5 |
| [Store](store.md) | `menus/store.conf` | The emerald in your minions | 5 |
| [Storage](storage.md) | `menus/storage.conf` | The chest on the panel | 6 |
| [Upgrade](upgrade.md) | `menus/upgrade.conf` | The bottle on the panel | 5 |
| [Fuel](fuel.md) | `menus/fuel.conf` | The coal on the panel | 4 |
| [Skin](skin.md) | `menus/skin.conf` | The chestplate on the panel | 5 |
| [Trusted](trusted.md) | `menus/trusted.conf` | The head on the panel | 4 |
| [Pickup](pickup.md) | `menus/pickup.conf` | The barrier on the panel | 3 |
| [Admin](admin.md) | `menus/admin.conf` | `/uxmminions minions <player>` | 6 |
| [Editor](editor.md) | `menus/editor.conf` | `/uxmminions editor` | 6 |
| [Property editor](editor.md#properties) | `menus/editor-type.conf` | A click on a type in the editor | 6 |
| [Editor lists](editor.md#lists) | `menus/editor-list.conf` | A list button | 6 |

## Shared rules

- **Bedrock.** Every verb a player needs sits on a left click: a Bedrock player taps a form button and sends no
  modifier, so a right or shift click is a shortcut and never the only way. Keep it that way when you move a button.
- **Refresh.** A window with `refresh { enabled = true, interval-ticks = 20 }` redraws itself from what the plugin last
  read, never from the database; `enabled = false` draws it once.
- **View gates.** `view = ["minions:permission:<node>"]` hides an item from a player without the node, and
  `view = ["minions:money:>= 5000"]` by what they hold. `open-requirement` gates the whole window.
- **Deleting.** Delete a window file and the window is gone; the command that opens it falls back where it can.

Every window is a file; see [Menu files](../config/menu-files.md). The sounds of every window are `menu.sounds` in
`config.conf`.

Related: [Menu files](../config/menu-files.md)
