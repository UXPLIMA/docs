---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmTeleportBow."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/uxmteleportbow` | `/tpbow`, `/uxmtb` | `give`, `reload`, `version`, `doctor`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmteleportbow.language` | Read this plugin in a language of your own. | true |
| `uxmteleportbow.admin.language` | Set or clear the language another player reads | op |
| `uxmteleportbow.admin.*` | Everything an operator does with this plugin | op |
| `uxmteleportbow.admin.give` | Give a teleport bow to a player | op |
| `uxmteleportbow.admin.doctor` | Ask how this plugin is | op |
| `uxmteleportbow.admin.version` | Read the version this server runs | op |
| `uxmteleportbow.admin.reload` | Read the configuration again | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmTeleportBow/` on first start and reads them again on reload:

- `commands.conf`
- `config.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `theme.conf`
