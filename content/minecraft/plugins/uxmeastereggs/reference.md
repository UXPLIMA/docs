---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmEasterEggs."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/eggs` | `/easteregg`, `/eastereggs` | `radar`, `top`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |
| `/uxmeastereggs` | `/uxmeggs`, `/eggsadmin` | `create`, `set`, `show`, `delete`, `reset`, `list`, `editor`, `tp`, `reload`, `version`, `doctor` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmeastereggs.language` | Read this plugin in a language of your own. | true |
| `uxmeastereggs.admin.language` | Set or clear the language another player reads | op |
| `uxmeastereggs.use` | Open the collection, switch the radar, read the leaderboard. | true |
| `uxmeastereggs.admin` | The whole admin branch. | op |
| `uxmeastereggs.admin.create` | Register the block you are looking at as an egg. | op |
| `uxmeastereggs.admin.delete` | Take an egg off the map, with every find of it. | op |
| `uxmeastereggs.admin.reset` | Take every find of a player away, and the player off every board. | op |
| `uxmeastereggs.admin.list` | List the eggs that are placed. | op |
| `uxmeastereggs.admin.editor` | Open the menu over every egg. | op |
| `uxmeastereggs.admin.tp` | Teleport to an egg. | op |
| `uxmeastereggs.admin.reload` | Re-read the configuration and the type files. | op |
| `uxmeastereggs.admin.doctor` | Ask how this plugin is. | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmEasterEggs/` on first start and reads them again on reload:

- `commands.conf`
- `config.conf`
- `menus/admin.conf`
- `menus/collection.conf`
- `menus/leaderboard.conf`
- `menus/picker.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `theme.conf`
- `types/common.conf`
- `types/golden.conf`
