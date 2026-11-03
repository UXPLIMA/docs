---
title: Operator commands
order: 203
description: Building, opening, editing and deleting arenas, erasing a runner, reload, doctor and version.
icon: shield
---

Every command here is a subcommand of `/uxmparkour` (aliases `/uxmpk`, `/parkouradmin`). Each needs its own node,
`op` by default; `uxmparkour.admin.*` grants them all.

## Commands

### Building an arena

| Command | Node | What it does |
|---|---|---|
| `/uxmparkour create <name> [world]` | `uxmparkour.admin.create` | Writes a new arena, empty and shut, in the world you stand in or the one named, and opens its editor |
| `/uxmparkour set <name> <piece> <value>` | `uxmparkour.admin.edit` | Sets one piece of an arena |
| `/uxmparkour show <name>` | `uxmparkour.admin.list` | Prints every piece, set or not, and why a complete arena cannot open |
| `/uxmparkour edit [name]` | `uxmparkour.admin.edit` | Opens the [editor](../menus/editor.md) for that arena, or the [arena list](../menus/arenas.md) |
| `/uxmparkour enable <name>` | `uxmparkour.admin.edit` | Opens the arena to players |
| `/uxmparkour disable <name>` | `uxmparkour.admin.edit` | Shuts the arena, so nobody joins it while it changes |
| `/uxmparkour delete <name>` | `uxmparkour.admin.delete` | Deletes the arena from `arenas.conf` |

- An arena name is one to 32 characters: lower-case letters, digits, `_` and `-`, starting with a letter or digit.
  It is stored in lower case. A name in use answers `arena.exists`; an illegal one, `arena.rejected`.
- `create` from the console needs the world: it stands nowhere.
- `enable` refuses an arena with a piece missing, naming the pieces, and one whose start block is outside its box.

### The pieces of `set`

| Piece | Value | Example |
|---|---|---|
| `start` | Three numbers, or `here` for the block under your feet | `/uxmparkour set sky start here` |
| `corner1`, `corner2` | Three numbers, or `here` | `/uxmparkour set sky corner2 160 170 40` |
| `heading` | `north`, `east`, `south` or `west`, or `here` for the way you face | `/uxmparkour set sky heading here` |
| `difficulty` | A difficulty id from `config.conf` | `/uxmparkour set sky difficulty hard` |
| `theme` | A theme id from `config.conf` | `/uxmparkour set sky theme glow` |
| `world` | A world name | `/uxmparkour set sky world skyblock` |

- A position given with `here` is taken in the world you stand in. A position in a world other than the arena's
  moves the arena there and drops the positions already set, because a box spans one world. `set <name> world`
  does the same.
- After each change the reply names the pieces still missing, or says the arena is ready, or saved when it is open.
- An edit that leaves an open arena unplayable, such as a corner moved so the start is outside the box, shuts it
  and says why.
- `here` needs a player; the console gets `arena.here-needs-a-player`.

### Players and records

| Command | Node | What it does |
|---|---|---|
| `/uxmparkour tp <name>` | `uxmparkour.admin.tp` | Moves you onto the start block of an arena |
| `/uxmparkour reset <player>` | `uxmparkour.admin.reset` | Takes a runner off the record and every board, in every period |

- `reset` finds the player by the name they last ran under, online or not. Their best, total jumps and total runs
  go back to zero on every server sharing the database; their theme and difficulty stay. Use it for a cheater or a
  test account that climbed a board. An unknown name answers `stats.unknown`.
- `tp` answers `arena.no-start` for an arena without a start block, and `arena.world-missing` when its world is not
  loaded.

### The plugin

| Command | Node | What it does |
|---|---|---|
| `/uxmparkour` | `uxmparkour.admin.list` | The same as `list` |
| `/uxmparkour list` | `uxmparkour.admin.list` | The arena list as a menu for a player, or one line for the console: each arena with `(open)`, `(shut)` or `(incomplete)` |
| `/uxmparkour reload` | `uxmparkour.admin.reload` | Reads `config.conf`, the menus, the messages and `arenas.conf` again, and says how long it took |
| `/uxmparkour doctor` | `uxmparkour.admin.doctor` | One line per health check: storage, windows, placeholders ([Installation](../getting-started/installation.md#checking-the-install)) |
| `/uxmparkour version` | `uxmparkour.admin.version` | The version this server runs |

A reload swaps the whole configuration at once. A value that is wrong stops the reload, keeps the last
configuration that worked, and answers `admin.reload-failed`; the console holds the reason.

<Callout type="warning" title="Disabling or deleting any arena ends every run">

`disable` and `delete` end every run on the server, in every arena, before they change the file. Every runner is
put back where they joined from with their inventory, and the runs count on the boards. Shut an arena when the
lobby is quiet.

</Callout>

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmparkour.admin.*` | op | Every node below |
| `uxmparkour.admin.create` | op | `create` |
| `uxmparkour.admin.edit` | op | `set`, `edit`, `enable`, `disable` |
| `uxmparkour.admin.delete` | op | `delete` |
| `uxmparkour.admin.list` | op | `list`, `show`, the bare `/uxmparkour` |
| `uxmparkour.admin.tp` | op | `tp` |
| `uxmparkour.admin.reset` | op | `reset` |
| `uxmparkour.admin.reload` | op | `reload` |
| `uxmparkour.admin.doctor` | op | `doctor` |
| `uxmparkour.admin.version` | op | `version` |
| `uxmparkour.admin.build` | op | Digging and building inside an arena while a run is in it ([Runs](../arenas/runs.md#protection)) |

Related: [Your first arena](../getting-started/first-arena.md), [Arena editor](../menus/editor.md),
[arenas.conf](../config/arenas-conf.md)
