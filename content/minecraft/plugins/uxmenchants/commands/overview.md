---
title: Overview
order: 301
description: Both commands at a glance, renaming them, and who sees what.
icon: list
---

| Command | Aliases | Subcommands |
|---|---|---|
| `/enchants` | `/enchantments`, `/ce` | `groups`, `info`, `list`, `limit`, `descriptions`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |
| `/uxmenchants` | `/uxme`, `/enchantsadmin` | `version`, `doctor`, `reload`, `editor`, `apply`, `remove`, `give`, `giverandom`, `export`, `strip`, `import` |

The bare `/enchants` opens the [wiki](../menus/wiki.md). The bare `/uxmenchants` says to add `help`, which lists the
subcommands the sender may use, page by page.

Names and aliases are the defaults from [commands.conf](../config/commands-conf.md); a change needs a restart.

## Who sees what

`uxmenchants.use` is given to everyone by default, so a player whose rank had it taken away still sees `/enchants` and
its subcommands and is told they may not use them. Every `uxmenchants.admin` node is operators only and hides its
subcommand from everybody else, and so do `lang set-player` and `lang reset-player`.

## From the console

`list`, `lang set-player`, `lang reset-player`, `version`, `doctor`, `reload`, `give`, `giverandom`, `strip` and
`import` run from the console. The rest need a player: the windows, the limit, the description toggle, a player's own
language, and the four operator commands that read the item in hand (`editor`, `apply`, `remove`, `export`).

Related: [Player commands](player.md), [Admin commands](admin.md)
