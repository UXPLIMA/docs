---
title: Overview
order: 301
description: The two roots, renaming them, what runs from the console, and who sees what.
icon: list-tree
---

uxmCrates has two commands.

| Root | Aliases | Page |
|---|---|---|
| `/crates` | `/crate`, `/cr` | [Player commands](player.md): the windows, opening, previews, keys, claims |
| `/uxmcrates` | `/cratesadmin`, `/uxmcr` | [Operator commands](admin.md), and the language commands |

## Renaming

Every name, alias and subcommand is set in [`commands.conf`](../config/commands-conf.md). The key on the left never
changes; the name on the right is yours. A subcommand set to `enabled = false` leaves the tree; a root set to
`enabled = false` is never registered. A change needs a restart: the server builds its command tree once, at start.

## Console

Every `/crates` subcommand needs a player. Every `/uxmcrates` subcommand runs from the console except `place`, `tool`,
`remove` and `editor`, which need a block or a window, and the player's own `lang set` and `lang reset`. From the
console, `/uxmcrates log` prints lines instead of opening a window.

## A player

A command that names a player takes the name of anyone online or anyone the server remembers, so keys and rerolls
can be given to a player who is away. `forceopen` is the exception: it needs the player online. A name the server has
never seen answers `error.unknown-player`.

## Permissions

`/crates` and its subcommands carry no node. A player who may not open a crate is refused by the crate itself: its
key, its `permission`, its requirements, its cooldown or its world, each with its own message. Every operator
subcommand carries its own `uxmcrates.admin.<action>` node, all `op` by default, and a sender without a subcommand's
node does not see it. See the [permission reference](../permissions/reference.md).

## Notes

- **The bare `/crates`** opens the [crate list](../menus/crates.md).
- **The bare `/uxmcrates`** says to add `help`, and needs `uxmcrates.admin.version`. The branch words `key`, `reset`
  and `reroll` answer the same way.
- **Every root has a help page.** `help` after a root lists the subcommands the sender may use, with a click that
  writes each one into the chat bar.

Related: [Player commands](player.md), [Operator commands](admin.md), [commands.conf](../config/commands-conf.md)
