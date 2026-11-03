---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmGlow."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/glow` | `/glowcolor`, `/glowcolour` | `set`, `cycle`, `off`, `colours`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |
| `/uxmglow` | `/uxmg`, `/glowadmin` | `reload`, `version`, `doctor` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmglow.language` | Read this plugin in a language of your own. | true |
| `uxmglow.admin.language` | Set or clear the language another player reads | op |
| `uxmglow.use` | Open the glow menu and turn your own glow on or off | true |
| `uxmglow.colour.*` | Every colour, including the ones a later release adds | op |
| `uxmglow.cycle` | Glow in a colour that changes rather than one that stays | op |
| `uxmglow.admin.*` | Everything an operator does with this plugin | op |
| `uxmglow.admin.doctor` | Ask how this plugin is | op |
| `uxmglow.admin.version` | Read the version this server runs | op |
| `uxmglow.admin.reload` | Read the configuration and the colours again | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmGlow/` on first start and reads them again on reload:

- `commands.conf`
- `config.conf`
- `menus/glow.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `theme.conf`
