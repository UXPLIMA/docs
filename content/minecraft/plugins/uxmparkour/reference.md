---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmParkour."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/parkour` | `/pk`, `/jump` | `join`, `leave`, `restart`, `themes`, `theme`, `difficulty`, `stats`, `top`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |
| `/uxmparkour` | `/uxmpk`, `/parkouradmin` | `reload`, `version`, `create`, `set`, `show`, `enable`, `disable`, `edit`, `delete`, `reset`, `list`, `tp`, `doctor` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmparkour.language` | Read this plugin in a language of your own. | true |
| `uxmparkour.admin.language` | Set or clear the language another player reads | op |
| `uxmparkour.play` | Run a course, and read your own times | true |
| `uxmparkour.admin.*` | Everything an operator does with this plugin | op |
| `uxmparkour.admin.create` | Make a course | op |
| `uxmparkour.admin.delete` | Take a course away | op |
| `uxmparkour.admin.reset` | Take a runner off the record and every board | op |
| `uxmparkour.admin.edit` | Change a course, its checkpoints and its rewards | op |
| `uxmparkour.admin.build` | Build inside a course area while it is protected | op |
| `uxmparkour.admin.list` | Read which courses there are | op |
| `uxmparkour.admin.tp` | Go to a course | op |
| `uxmparkour.admin.doctor` | Ask how this plugin is | op |
| `uxmparkour.admin.version` | Read the version this server runs | op |
| `uxmparkour.admin.reload` | Read the configuration and the courses again | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmParkour/` on first start and reads them again on reload:

- `arenas.conf`
- `commands.conf`
- `config.conf`
- `menus/arena-editor.conf`
- `menus/arenas.conf`
- `menus/themes.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `theme.conf`
