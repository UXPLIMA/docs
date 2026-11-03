---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmLang."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/lang` | `/language`, `/dil` | `list`, `set`, `reset` |
| `/uxmlang` | `/langadmin` | `reload`, `version`, `get`, `set`, `reset`, `doctor` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmlang.use` | Choose the language you read this server in | true |
| `uxmlang.admin.*` | Everything an operator does with this plugin | op |
| `uxmlang.admin.get` | Read which language a player has chosen | op |
| `uxmlang.admin.set` | Set the language a player reads | op |
| `uxmlang.admin.doctor` | Ask how this plugin is | op |
| `uxmlang.admin.version` | Read the version this server runs | op |
| `uxmlang.admin.reload` | Read the configuration and the language files again | op |

## Placeholders

The plugin registers no PlaceholderAPI placeholders.

## Files

The plugin writes these into `plugins/uxmLang/` on first start and reads them again on reload:

- `commands.conf`
- `config.conf`
- `languages.conf`
- `menus/language.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `theme.conf`
