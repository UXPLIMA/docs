---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmJoinItems."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/uxmjoinitems` | `/joinitems`, `/uxmji` | `give`, `giveall`, `clear`, `reload`, `version`, `doctor`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmjoinitems.language` | Read this plugin in a language of your own. | true |
| `uxmjoinitems.admin.language` | Set or clear the language another player reads | op |
| `uxmjoinitems.admin.*` | Everything an operator does with this plugin | op |
| `uxmjoinitems.admin.give` | Give a player the items they get on join | op |
| `uxmjoinitems.admin.giveall` | Give every player online the items they get on join | op |
| `uxmjoinitems.admin.clear` | Take the join items off a player | op |
| `uxmjoinitems.admin.doctor` | Ask how this plugin is | op |
| `uxmjoinitems.admin.version` | Read the version this server runs | op |
| `uxmjoinitems.admin.reload` | Read the configuration again | op |

## Placeholders

The plugin registers no PlaceholderAPI placeholders.

## Files

The plugin writes these into `plugins/uxmJoinItems/` on first start and reads them again on reload:

- `commands.conf`
- `config.conf`
- `events/first-join.conf`
- `events/join.conf`
- `events/respawn.conf`
- `events/world-change.conf`
- `items/cosmetics.conf`
- `items/lobby.conf`
- `items/selector.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `theme.conf`
