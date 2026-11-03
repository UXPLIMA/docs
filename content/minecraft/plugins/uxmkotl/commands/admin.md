---
title: Operator commands
order: 303
description: Every /uxmkotl subcommand, with its arguments, what it does and the permission it needs.
icon: shield
---

Every operator command is under `/uxmkotl`, alias `/kotladmin`. Each has its own node, all `op` by default.

## Building arenas

| Command | Permission | What it does |
|---|---|---|
| `/uxmkotl create <arena> [world]` | `uxmkotl.admin.create` | Writes a new arena, empty and switched off, in the world named or the one you stand in, and opens the [editor](../menus/editor.md) on it |
| `/uxmkotl set <arena> <piece> <value>` | `uxmkotl.admin.edit` | Sets one piece or switch of an arena. See below |
| `/uxmkotl show <arena>` | `uxmkotl.admin.list` | Prints every piece and switch, one per line, set or not |
| `/uxmkotl edit <arena>` | `uxmkotl.admin.edit` | Opens the editor on the arena. Player only |
| `/uxmkotl enable <arena>` | `uxmkotl.admin.enable` | Switches the arena on. Refused, with what is missing, until every piece is set and the plate is inside the box |
| `/uxmkotl disable <arena>` | `uxmkotl.admin.disable` | Switches the arena off. Nobody new gets in; players already inside keep playing until they leave |
| `/uxmkotl delete <arena>` | `uxmkotl.admin.delete` | Deletes the arena and its file. Refused with `error.arena-in-use` while a player is inside |
| `/uxmkotl list` | `uxmkotl.admin.list` | For a player, opens the [arena list](../menus/list.md). From the console, or with no arena yet, writes the arenas with on or off |
| `/uxmkotl tp <arena>` | `uxmkotl.admin.tp` | Teleports you to the arena's spawn. Player only |

An arena id is one to 32 characters, lower-case letters, digits and `-`, starting with a letter or a digit. Anything
else answers `error.invalid-id`.

### set

`/uxmkotl set <arena> <piece> <value>` takes the rest of the line as the value.

| Piece | Value | Sets |
|---|---|---|
| `corner1`, `corner2` | `here`, or `<x> <y> <z>` | A corner of the box. `here` is the block your feet are in |
| `plate` | `here`, or `<x> <y> <z>` | The block a king stands in |
| `spawn` | `here`, or `<x> <y> <z>` | Where a joining player is put. `here` keeps where you face; numbers are the middle of that block, facing south |
| `exit` | `here`, or `<x> <y> <z>` | Where a leaving player is put, in the same way. It may be in another world |
| `world` | a world name | Moves the arena to that world. The corners, the plate and the spawn are dropped; the exit stays |
| `name` | any text | The name players read in messages, such as *You joined The Hill*. It is plain text: tags are shown as written. The id does not change |
| `join-on-enter` | `true`/`yes`/`on` or `false`/`no`/`off` | Whether walking into the box joins |
| `clear-inventory` | the same | Whether the items are put away on entry |
| `clear-effects` | the same | Whether the potion effects are removed on entry |
| `mode` | `survival`, `creative`, `adventure` or `spectator` | The game mode a player is put in |

Numbers are whole numbers in the arena's world. A block piece or the spawn set with `here` in another world moves the
whole arena to that world and drops the pieces of the old one, so a box never has a corner in each of two worlds.

The reply says what is still missing (`arena.updated`), that every piece is set (`arena.ready` on a switched off
arena, `arena.saved` on an open one), or, when the change broke an open arena, that it was switched off and why
(`arena.shut-incomplete`, `arena.shut-fault`).

```
/uxmkotl set hill plate 23 68 -43
/uxmkotl set hill name The Hill
/uxmkotl set hill mode adventure
```

### show

```
hill
  name hill
  world world
  corner1 18 61 -48
  corner2 28 76 -38
  plate 23 68 -43
  spawn 23 62 -39 world
  exit 23 62 -33 world
  enabled true
  join-on-enter true
  clear-inventory true
  clear-effects true
  mode adventure
```

A place is shown as the block it stands in and its world. A piece that is not set reads *not set*. When every piece is
set but the plate is outside the box, a last line says so.

## Players

| Command | Permission | What it does |
|---|---|---|
| `/uxmkotl reset <player>` | `uxmkotl.admin.reset` | Takes a player off the record and off every board, in every period, by the name they played under. The rows go on every server sharing the database |

`reset` answers `stats.unknown` for a name that never played, and `stats.reset` when the rows are gone.

## The plugin

| Command | Permission | What it does |
|---|---|---|
| `/uxmkotl` | `uxmkotl.admin.version` | The same as `list` |
| `/uxmkotl reload` | `uxmkotl.admin.reload` | Reads the configuration, the messages, the theme, the window files and the arena files again. See [Configuration](../config/index.md#reloading) |
| `/uxmkotl doctor` | `uxmkotl.admin.doctor` | Checks the storage, the windows and PlaceholderAPI, one line each. See [Installation](../getting-started/installation.md#checking-the-install) |
| `/uxmkotl version` | `uxmkotl.admin.version` | Prints the version this server runs |

## Notes

- **Players inside keep the old arena.** A player plays on the arena as it was when they got in. An edit, a reload
  or `disable` reaches them when they leave and come back.
- **The ring needs the arena on at a start or a reload.** The particle ring over a king is drawn for the arenas
  that were switched on when the server started or last reloaded. After `enable`, run `/uxmkotl reload` to draw it.

Related: [Overview](overview.md), [Arena files](../config/arena-files.md), [Arena editor](../menus/editor.md)
