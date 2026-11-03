---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmPlayerHider."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/hide` | `/playerhider`, `/ph` | `state`, `status`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |
| `/uxmplayerhider` | `/uxmph`, `/hideadmin` | `reload`, `version`, `doctor` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmplayerhider.language` | Read this plugin in a language of your own. | true |
| `uxmplayerhider.admin.language` | Set or clear the language another player reads | op |
| `uxmplayerhider.use` | Hide the other players, or show them again | true |
| `uxmplayerhider.cooldown.bypass` | Toggle again without waiting out the cooldown | op |
| `uxmplayerhider.important` | Stay visible to a player who has hidden everybody but the important ones | op |
| `uxmplayerhider.admin.*` | Everything an operator does with this plugin | op |
| `uxmplayerhider.admin.doctor` | Ask how this plugin is | op |
| `uxmplayerhider.admin.version` | Read the version this server runs | op |
| `uxmplayerhider.admin.reload` | Read the configuration again | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmPlayerHider/` on first start and reads them again on reload:

- `commands.conf`
- `config.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `theme.conf`
