---
title: Overview
order: 201
description: The two roots, renaming them, what runs from the console, and who sees what.
icon: list-tree
---

uxmCosmetics has two commands.

| Root | Aliases | Page |
|---|---|---|
| `/cosmetics` | `/cosmetic`, `/wardrobe` | [Player commands](player.md): the wardrobe, the boxes, equipping, the language |
| `/uxmcosmetics` | `/uxmcos`, `/cosmeticsadmin` | [Operator commands](admin.md) |

## Renaming

Every name, alias and subcommand is set in [`commands.conf`](../config/commands-conf.md). A change needs a restart.

## Console

Every `/cosmetics` subcommand needs a player, except `lang set-player` and `lang reset-player`. Every `/uxmcosmetics`
subcommand runs from the console; `station add` from the console needs the block written out.

## Permissions

| Node | Default | Who sees what |
|---|---|---|
| `uxmcosmetics.use` | everyone | `/cosmetics` and its wardrobe subcommands. A player who lost it still sees them and is told `command.no-permission` |
| `uxmcosmetics.language` | everyone | `/cosmetics lang`, `lang set`, `lang reset`. The same: visible, and refused out loud |
| `uxmcosmetics.admin.*` nodes | op | Each operator subcommand. A sender without the node does not see it |

See the [permission reference](../permissions/reference.md).

## Notes

- **The bare `/cosmetics`** opens the [wardrobe](../menus/wardrobe.md).
- **The bare `/uxmcosmetics`** says to add `help`, and needs `uxmcosmetics.admin.version`.
- **The bare `/uxmcosmetics station`** lists the stations, as `station list` does.
- **Every root has a help page.** `help` after a root lists the subcommands the sender may use.

Related: [Player commands](player.md), [Operator commands](admin.md), [commands.conf](../config/commands-conf.md)
