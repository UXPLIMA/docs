---
title: Operator commands
order: 303
description: Every /uxmeastereggs subcommand, with its arguments, what it does and the permission it needs.
icon: shield
---

Every operator command is under `/uxmeastereggs`, aliases `/uxmeggs` and `/eggsadmin`. The whole root needs
`uxmeastereggs.admin`; most subcommands need their own node as well. `uxmeastereggs.admin` grants every one of them,
and all are `op` by default.

## Placing eggs

| Command | Permission | What it does |
|---|---|---|
| `/uxmeastereggs create` | `admin.create` | Opens the [type picker](../menus/picker.md) for the block you look at. Player only |
| `/uxmeastereggs create <type> [block]` | `admin.create` | Registers the block as an egg of that type. With no block, the one you look at |
| `/uxmeastereggs set <egg> type <type>` | `admin.create` | Changes the egg's type. Every find of it stays |
| `/uxmeastereggs set <egg> position <block>` | `admin.create` | Moves the egg to another block, `here` for the one you look at. Every find of it stays |
| `/uxmeastereggs delete [egg]` | `admin.delete` | Takes the egg off the map, and every find of it. With no egg, the one you look at |

`create` answers `admin.placed` with the type, the position and the new id; `admin.occupied` when an egg already sits
on that block; `admin.type-unknown` for a type that has no file; `admin.look-at-block` when you look at nothing
within six blocks. The block itself is never changed: build it first, then register it.

```
/uxmeastereggs create golden
/uxmeastereggs create common 12 70 -4 world
/uxmeastereggs set 3f2a91c0 type golden
/uxmeastereggs set 3f2a91c0 position here
/uxmeastereggs delete 3f2a91c0
```

`delete` removes the finds of the egg from every player, so nobody keeps one of an egg that is gone. The board rows
those finds made are kept.

## Looking at eggs

| Command | Permission | What it does |
|---|---|---|
| `/uxmeastereggs list` | `admin.list` | Every egg: its short id, type, world and position |
| `/uxmeastereggs show <egg>` | `admin.list` | One egg: its type, world and position |
| `/uxmeastereggs editor` | `admin.editor` | Opens the [placed eggs](../menus/placed-eggs.md) window. Player only |
| `/uxmeastereggs tp <egg>` | `admin.tp` | Stands you on top of the egg. Player only |

```
There are 3 eggs:
3f2a91c0 common in world 12 70 -4
a07c55d2 golden in world -40 81 17
...
```

## Players

| Command | Permission | What it does |
|---|---|---|
| `/uxmeastereggs reset <player>` | `admin.reset` | Takes every find of the player away and the player off every board, by the name they last played under. Runs off the main thread and answers when it is done |

A name nobody played under answers `admin.player-unknown`.

## The plugin

| Command | Permission | What it does |
|---|---|---|
| `/uxmeastereggs` | `uxmeastereggs.admin` | The same as `list` |
| `/uxmeastereggs reload` | `admin.reload` | Reads `config.conf`, the type files, the messages, the theme and the window files again. See [Configuration](../config/index.md#reloading) |
| `/uxmeastereggs doctor` | `admin.doctor` | Checks the storage, the windows and PlaceholderAPI, one line each |
| `/uxmeastereggs version` | `uxmeastereggs.admin` | Prints the version this server runs |

The nodes above are written short: `admin.create` is `uxmeastereggs.admin.create`.

Related: [Overview](overview.md), [Eggs and finds](../hunting/overview.md), [Permissions](../permissions/reference.md)
