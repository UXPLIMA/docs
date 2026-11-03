---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmLevels."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/levels` | `/lvl`, `/level` | `level`, `level of`, `top`, `prestige`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |
| `/uxmlevels` | `/levelsadmin` | `reload`, `version`, `give`, `take`, `set`, `reset`, `boost`, `unboost`, `doctor` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmlevels.language` | Read this plugin in a language of your own. | true |
| `uxmlevels.admin.language` | Set or clear the language another player reads | op |
| `uxmlevels.use` | Read your own level and open the windows this plugin draws | true |
| `uxmlevels.track.*` | Earn on every track, including the ones an operator adds later | true |
| `uxmlevels.others` | Read another player's level | op |
| `uxmlevels.prestige` | Spend your levels to start again with a badge | op |
| `uxmlevels.admin.*` | Everything an operator does with this plugin | op |
| `uxmlevels.admin.give` | Give a player experience | op |
| `uxmlevels.admin.take` | Take experience off a player | op |
| `uxmlevels.admin.set` | Set a player's level outright | op |
| `uxmlevels.admin.reset` | Start a player again on every track and take them off every board | op |
| `uxmlevels.admin.boost` | Run a multiplier for a while | op |
| `uxmlevels.admin.doctor` | Ask how this plugin is | op |
| `uxmlevels.admin.version` | Read the version this server runs | op |
| `uxmlevels.admin.reload` | Read the configuration and the tracks again | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmLevels/` on first start and reads them again on reload:

- `commands.conf`
- `config.conf`
- `events/combat.conf`
- `events/general.conf`
- `events/mining.conf`
- `menus/main.conf`
- `menus/prestige.conf`
- `menus/rewards.conf`
- `menus/top.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `theme.conf`
- `tracks/combat.conf`
- `tracks/global.conf`
- `tracks/mining.conf`
