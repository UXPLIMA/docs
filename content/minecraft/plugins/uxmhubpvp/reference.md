---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmHubPvP."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/pvp` | `/fight` | `stats`, `top`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |
| `/uxmhubpvp` | `/hubpvp`, `/pvpadmin` | `reload`, `version`, `doctor`, `reset` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmhubpvp.language` | Read this plugin in a language of your own. | true |
| `uxmhubpvp.admin.language` | Set or clear the language another player reads | op |
| `uxmhubpvp.use` | Turn your own PvP on, or off | true |
| `uxmhubpvp.stats` | Read your own record | true |
| `uxmhubpvp.top` | Read the leaderboard | true |
| `uxmhubpvp.bypass.commands` | Type a command while a fight is running | op |
| `uxmhubpvp.admin.*` | Everything an operator does with this plugin | op |
| `uxmhubpvp.admin.doctor` | Ask how this plugin is | op |
| `uxmhubpvp.admin.version` | Read the version this server runs | op |
| `uxmhubpvp.admin.reload` | Read the configuration again | op |
| `uxmhubpvp.admin.reset` | Take a player off the record and every board | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmHubPvP/` on first start and reads them again on reload:

- `commands.conf`
- `config.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `theme.conf`
