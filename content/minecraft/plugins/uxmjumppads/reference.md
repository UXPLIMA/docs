---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmJumpPads."
---

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/uxmjumppads set <type>` | `uxmjumppads.admin.edit` | Makes the block you stand on a pad of that type, throwing the way you face. |
| `/uxmjumppads remove` | `uxmjumppads.admin.edit` | Takes the pad off the block you stand on. |
| `/uxmjumppads list` | `uxmjumppads.admin.edit` | Lists the pads set on single blocks. |
| `/uxmjumppads reload` | `uxmjumppads.admin.reload` | Reads the configuration and the messages again. |
| `/uxmjumppads version` | `uxmjumppads.admin.version` | Shows the running version. |
| `/uxmjumppads doctor` | `uxmjumppads.admin.doctor` | Checks the pad types and the pads set on blocks. |
| `/jumppad lang` | `uxmjumppads.language` | Shows the language you read. |
| `/jumppad lang set <language>` | `uxmjumppads.language` | Chooses your language. |
| `/jumppad lang reset` | `uxmjumppads.language` | Goes back to the client or default language. |
| `/jumppad lang set-player <player> <language>` | `uxmjumppads.admin.language` | Chooses another player's language. |
| `/jumppad lang reset-player <player>` | `uxmjumppads.admin.language` | Clears another player's choice. |

`/uxmjumppads` also answers to `/uxmjp` and `/jumppadadmin`, and `/jumppad` to `/jumppads`. Every name is in
`commands.conf`.

## Permissions

| Permission | Default |
|---|---|
| `uxmjumppads.language` | everyone |
| `uxmjumppads.admin.*` | op, grants every `uxmjumppads.admin` permission |
| `uxmjumppads.admin.edit` | op |
| A pad type's own `permission` | whatever the server grants; empty means everyone |

## Placeholders

The plugin registers no PlaceholderAPI placeholders.
