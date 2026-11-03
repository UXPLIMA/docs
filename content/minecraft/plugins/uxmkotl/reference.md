---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmKotl."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/kotl` | `/ladder` | `join`, `leave`, `stats`, `top`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |
| `/uxmkotl` | `/kotladmin` | `create`, `set`, `show`, `edit`, `enable`, `disable`, `delete`, `reset`, `list`, `tp`, `reload`, `version`, `doctor` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmkotl.language` | Read this plugin in a language of your own. | true |
| `uxmkotl.admin.language` | Set or clear the language another player reads | op |
| `uxmkotl.play` | Join the arena and fight for the crown | true |
| `uxmkotl.admin.*` | Everything an operator does with this plugin | op |
| `uxmkotl.admin.create` | Make an arena | op |
| `uxmkotl.admin.delete` | Take an arena away | op |
| `uxmkotl.admin.reset` | Take a player off the record and every board | op |
| `uxmkotl.admin.edit` | Change an arena, its region and its spawns | op |
| `uxmkotl.admin.enable` | Open an arena to players | op |
| `uxmkotl.admin.disable` | Close an arena | op |
| `uxmkotl.admin.list` | Read which arenas there are and what state each is in | op |
| `uxmkotl.admin.tp` | Go to an arena | op |
| `uxmkotl.admin.doctor` | Ask how this plugin is | op |
| `uxmkotl.admin.version` | Read the version this server runs | op |
| `uxmkotl.admin.reload` | Read the configuration and the arenas again | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmKotl/` on first start and reads them again on reload:

- `commands.conf`
- `config.conf`
- `menus/editor.conf`
- `menus/list.conf`
- `menus/stats.conf`
- `menus/top.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `theme.conf`
