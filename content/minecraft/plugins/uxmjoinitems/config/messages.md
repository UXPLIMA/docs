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
| `<tag:'ITEMS'>` | The plugin's category word |
| `<etag:'ERROR'>` | An error |
| `<h:'...'>` | A header |

| Block | Values | Said when |
|---|---|---|
| `admin.version`, `usage`, `reloaded`, `reload-failed` | `<version>`, `<items>`, `<ms>` | The operator commands |
| `admin.item-given`, `items-given`, `items-cleared` | `<item>`, `<count>`, `<player>` | `give`, `giveall`, `clear` |
| `admin.unknown-item`, `target-offline`, `outside-region`, `guarded`, `no-room` | `<item>`, `<player>` | Why `give` did not land |
| `admin.doctor.*` | `<check>`, `<said>` | `/uxmjoinitems doctor` |
| `item.cooldown` | `<seconds>` | A click within the cooldown |
| `item.description`, `item.navigation` | | Shared tooltip lines |
| `item.selector.*`, `item.lobby.*`, `item.cosmetics.*` | | The shipped items' tooltips: `title`, `crumb`, `line-1`, `line-2`, `action` |
| `event.join.welcome` | `<player>` | The shipped join action |
| `command.*` | | The command layer: help pages, rejected arguments |
| `language.*` | `<language>`, `<languages>`, `<player>`, `<input>` | `/uxmjoinitems lang` |

Add blocks of your own for your own items, such as `item.profile.title`, in every language file.

Related: [Languages](languages.md), [theme.conf](theme.md)
