---
title: Messages
order: 605
description: How a line is written, the prefixes, and every block of keys with the values each fills in.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`. A line is MiniMessage and names roles, not colours; the
colours are in [theme.conf](theme.md). A key that is missing falls back to the default the code carries.

| Prefix | Draws |
|---|---|
| `<tag:'LOBBY'>` | The plugin's category word |
| `<etag:'ERROR'>` | An error |
| `<h:'...'>` | A header |

| Block | Values | Said when |
|---|---|---|
| `admin.version`, `usage`, `reloaded`, `reload-failed` | `<version>`, `<ms>` | The operator commands |
| `admin.doctor.*` | `<check>`, `<said>` | `/uxmmultilobby doctor` |
| `connecting`, `already-there`, `unreachable`, `full`, `not-permitted`, `cooldown`, `no-such-lobby`, `nothing-available` | `<lobby>`, `<players>`, `<maximum>`, `<seconds>`, `<name>` | Each has `chat`, `title`, `subtitle` and `action-bar`, named by the outcome lists |
| `menu.title`, `missing`, `lore.*`, `button.*` | `<menu>` | The window |
| `menu.lobby.*` | `<lobby>`, `<players>`, `<maximum>` | A lobby's tile: description, players, the four status facts |
| `menu.action.*` | | The closing line of a tile, by state |
| `menu.best.*` | | The best-lobby button |
| `tool.name`, `tool.lore` | | The hotbar tool |
| `lobby.<id>.name` | | Each lobby's name |
| `command.*` | | The command layer: help pages, rejected arguments |
| `language.*` | `<language>`, `<languages>`, `<player>`, `<input>` | `/lobby lang` |

Related: [Languages](languages.md), [theme.conf](theme.md)
