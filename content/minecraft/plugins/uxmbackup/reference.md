---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmBackup."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/uxmbackup` | `/backup`, `/uxmb` | `version`, `reload`, `run`, `list`, `info`, `verify`, `prune`, `storages`, `status`, `menu`, `restore`, `hash`, `doctor`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmbackup.language` | Read this plugin in a language of your own. | true |
| `uxmbackup.*` | Everything an operator does with this plugin | op |
| `uxmbackup.menu` | Open the backup window | op |
| `uxmbackup.run` | Take a backup now | op |
| `uxmbackup.list` | Read what backups there are | op |
| `uxmbackup.status` | Read what the last backup did and when the next one is | op |
| `uxmbackup.verify` | Check that a backup is whole and can be read | op |
| `uxmbackup.restore` | Put a backup back, which overwrites what is there now | op |
| `uxmbackup.prune` | Delete the backups a retention rule has aged out | op |
| `uxmbackup.doctor` | Ask how this plugin is | op |
| `uxmbackup.reload` | Read the configuration again | op |
| `uxmbackup.player-language` | Set or clear the language another player reads | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmBackup/` on first start and reads them again on reload:

- `commands.conf`
- `config.conf`
- `menus/archives.conf`
- `messages/messages_en.conf`
- `messages/messages_tr.conf`
- `theme.conf`
