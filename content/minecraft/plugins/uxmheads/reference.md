---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmHeads."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/uxmheads` | `/heads`, `/uxmh` | `preview`, `refresh`, `welcome`, `reload`, `version`, `doctor`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmheads.language` | Choose the language you read this plugin in | true |
| `uxmheads.admin.*` | Everything an operator does with this plugin | op |
| `uxmheads.admin.preview` | See any player's face as it is drawn in chat | op |
| `uxmheads.admin.refresh` | Read a player's skin again | op |
| `uxmheads.admin.welcome` | Send a player the welcome again | op |
| `uxmheads.admin.doctor` | Ask how this plugin is | op |
| `uxmheads.admin.version` | Read the version this server runs | op |
| `uxmheads.admin.reload` | Read the configuration again | op |
| `uxmheads.admin.language` | Set or clear the language another player reads | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmHeads/` on first start and reads them again on reload:

- `commands.conf`
- `config.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `theme.conf`
