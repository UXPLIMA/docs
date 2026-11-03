---
title: Overview
order: 201
description: Every command at a glance, renaming them, and who sees what.
icon: list
---

| Command | Aliases | Subcommands | Node |
|---|---|---|---|
| `/lobby` | `/lobbies`, `/hub` | `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` | `uxmmultilobby.use` |
| `/quicklobby` | `/ql` | | `uxmmultilobby.use` |
| `/uxmmultilobby` | `/uxmml`, `/lobbyadmin` | `reload`, `version`, `doctor`, `help` | `uxmmultilobby.admin.*` nodes |

Names and aliases are the defaults from [commands.conf](../config/commands-conf.md). A server that already has a
`/lobby` or `/hub`, such as one that sends players to a hub, renames this one there.

`uxmmultilobby.use` and `uxmmultilobby.language` are given to everyone, so a player whose rank had one taken away still
sees the command and is told they may not use it. The `uxmmultilobby.admin` nodes are operators only and hide their
subcommands from everybody else.

Related: [Player commands](player.md), [Admin commands](admin.md)
