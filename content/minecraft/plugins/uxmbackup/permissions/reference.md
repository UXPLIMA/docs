---
title: Reference
order: 601
description: Every permission node of uxmBackup with its default.
icon: list-checks
---

| Permission | What it allows | Default |
|---|---|---|
| `uxmbackup.language` | `/uxmbackup lang`, `lang set`, `lang reset` | everyone |
| `uxmbackup.*` | Declared as the family; it lists no children, so grant the nodes below | op |
| `uxmbackup.menu` | Open the archives window | op |
| `uxmbackup.run` | Take a backup now | op |
| `uxmbackup.list` | `list` and `info` | op |
| `uxmbackup.status` | `/uxmbackup`, `version`, `storages`, `status` | op |
| `uxmbackup.verify` | Check that a backup is whole, by command or in the window | op |
| `uxmbackup.restore` | `restore` and `hash`, from the console only | op |
| `uxmbackup.prune` | Delete the backups a retention rule has aged out | op |
| `uxmbackup.doctor` | Ask how this plugin is | op |
| `uxmbackup.reload` | Read the configuration again | op |
| `uxmbackup.player-language` | Set or clear the language another player reads | op |

Everything except `uxmbackup.language` is for operators. A restore needs the console whatever a player holds.

Related: [Operator commands](../commands/admin.md)
