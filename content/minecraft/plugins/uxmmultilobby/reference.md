---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmMultiLobby."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/lobby` | `/lobbies`, `/hub` | `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |
| `/quicklobby` | `/ql` | `reload`, `version`, `doctor` |
| `/uxmmultilobby` | `/uxmml`, `/lobbyadmin` | `reload`, `version`, `doctor` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmmultilobby.language` | Read this plugin in a language of your own. | true |
| `uxmmultilobby.admin.language` | Set or clear the language another player reads | op |
| `uxmmultilobby.use` | Open the lobby window and travel to a lobby you may enter | true |
| `uxmmultilobby.admin.*` | Everything an operator does with this plugin | op |
| `uxmmultilobby.admin.doctor` | Ask how this plugin is | op |
| `uxmmultilobby.admin.version` | Read the version this server runs | op |
| `uxmmultilobby.admin.reload` | Read the configuration and the lobbies again | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmMultiLobby/` on first start and reads them again on reload:

- `commands.conf`
- `config.conf`
- `lobbies.conf`
- `menus/lobbies.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `theme.conf`
