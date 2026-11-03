---
title: Overview
order: 201
description: The six commands, their aliases, and how a missing permission is answered.
icon: terminal
---

| Command | Aliases | For |
|---|---|---|
| [`/shop`](shop.md) | `/shops` | The catalogue and languages |
| [`/sell`](shop.md#sell) | | Selling to the catalogue |
| [`/value`](shop.md#value) | `/worth` | What an item is worth |
| [`/myshop`](myshop.md) | `/ms` | Player shops |
| [`/uxmshop`](admin.md) | `/shopadmin` | Operators |
| [`/shopedit`](editor.md) | | The in-game editor, while `editor.enabled` is true |

Every name and alias is in [commands.conf](../config/commands-conf.md); a change there needs a restart.

A player node is held by everybody, so its command stays visible and a player without it is refused with
`command.no-permission`. An operator node hides its command from anybody who lacks it.

Related: [Permissions](../permissions/reference.md)
