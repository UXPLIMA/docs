---
title: Reference
order: 701
description: Every permission node of uxmMultiLobby with its default and what it allows.
icon: list-checks
---

## Players

| Node | Default | Allows |
|---|---|---|
| `uxmmultilobby.use` | everyone | `/lobby`, `/lobby <id>`, `/quicklobby`, the window and the hotbar tool |
| `uxmmultilobby.language` | everyone | `/lobby lang`, `lang set`, `lang reset` |

A player who lost one of these still sees the command and is told `command.no-permission`.

## Lobbies

Each lobby's `permission` in `lobbies.conf` is a node of your choosing: the shipped `lobby-private` names
`uxmmultilobby.lobby.private`. The plugin does not declare these, so they default to operators only; grant them to the
ranks that may enter.

## Operators

| Node | Default | Allows |
|---|---|---|
| `uxmmultilobby.admin.*` | op | Declared as the family. It lists no children: grant the nodes below, or use a permission plugin's wildcard |
| `uxmmultilobby.admin.reload` | op | `/uxmmultilobby reload` |
| `uxmmultilobby.admin.version` | op | `/uxmmultilobby` and `version` |
| `uxmmultilobby.admin.doctor` | op | `/uxmmultilobby doctor` |
| `uxmmultilobby.admin.language` | op | `/lobby lang set-player` and `reset-player` |

Related: [Commands](../commands/overview.md)
