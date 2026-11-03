---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmMinions."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/minions` | `/minion`, `/mn` | `list`, `report`, `trust`, `untrust`, `name`, `consume`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |
| `/uxmminions` | `/minionsadmin` | `give`, `find`, `open`, `minions`, `remove`, `restore`, `slots`, `ledger`, `migrate`, `editor`, `reload`, `doctor`, `version` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmminions.limits.bypass` | Place minions past the limit a node sets | op |
| `uxmminions.admin.*` | Everything an operator does with this plugin | op |
| `uxmminions.admin.doctor` | Ask how this plugin is | op |
| `uxmminions.admin.version` | Read the version this server runs | op |
| `uxmminions.admin.reload` | Read the configuration and the content again | op |
| `uxmminions.admin.language` | Set or clear the language another player reads | op |
| `uxmminions.admin.editor` | Build and change minion types from inside the game | op |
| `uxmminions.admin.give` | Give a player a minion | op |
| `uxmminions.admin.remove` | Take a minion out of the world | op |
| `uxmminions.admin.find` | Find where a player's minions stand | op |
| `uxmminions.admin.open` | Open somebody else's minion | op |
| `uxmminions.admin.minions` | List the minions a player has placed | op |
| `uxmminions.admin.slots` | Read and change how many a player may place | op |
| `uxmminions.admin.ledger` | Read what a player's minions have earned | op |
| `uxmminions.admin.restore` | Put a minion back after it was lost | op |
| `uxmminions.admin.migrate` | Read content in from another plugin | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmMinions/` on first start and reads them again on reload:

- `commands.conf`
- `config.conf`
- `editor/new-type.conf`
- `fuel.conf`
- `hologram.conf`
- `menus/admin.conf`
- `menus/editor-list.conf`
- `menus/editor-type.conf`
- `menus/editor.conf`
- `menus/fuel.conf`
- `menus/my-minions.conf`
- `menus/panel.conf`
- `menus/pickup.conf`
- `menus/pocket.conf`
- `menus/skin.conf`
- `menus/storage.conf`
- `menus/store.conf`
- `menus/trusted.conf`
- `menus/upgrade.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `prices.conf`
- `skins.conf`
- `theme.conf`
- `tool.conf`
- `types/collector.conf`
- `types/farmer.conf`
- `types/feeder.conf`
- `types/fisher.conf`
- `types/healer.conf`
- `types/lumberjack.conf`
- `types/miner.conf`
- `types/seller.conf`
- `types/slayer.conf`
- `types/spawner.conf`
