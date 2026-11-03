---
title: Operator commands
order: 303
description: Every /uxmjumppads subcommand, with its arguments, what it does and the permission it needs.
icon: shield
---

Every operator command is under `/uxmjumppads`, aliases `/uxmjp` and `/jumppadadmin`.

| Command | Permission | What it does |
|---|---|---|
| `/uxmjumppads set <type>` | `uxmjumppads.admin.edit` | Makes the block you stand on a pad of that type, throwing the way you face. Player only |
| `/uxmjumppads remove` | `uxmjumppads.admin.edit` | Takes the set pad off the block you stand on. Player only |
| `/uxmjumppads list` | `uxmjumppads.admin.edit` | Every set pad: its type, world, block and direction |
| `/uxmjumppads reload` | `uxmjumppads.admin.reload` | Reads `config.conf`, the messages and the theme again. See [Configuration](../config/index.md#reloading) |
| `/uxmjumppads doctor` | `uxmjumppads.admin.doctor` | Checks the pad types and the set pads, one line each |
| `/uxmjumppads version` | `uxmjumppads.admin.version` | Prints the version this server runs |
| `/uxmjumppads` | `uxmjumppads.admin.version` | Points to `/uxmjumppads help` |

## set

`set` reads the block at your feet; when your feet are in air, as on a full block, it takes the block under you. So
standing on a plate sets the plate, and standing on a slime block sets the slime block. The block can be any block:
it need not be the type's `block` on its `under`.

```
/uxmjumppads set classic
The block at world 12 64 -3 is a classic pad, throwing at 180 degrees.
```

The direction is your yaw in whole degrees, as the debug screen shows it: 0 south, 90 west, 180 north, 270 east. A
`set` on a block that is a set pad already replaces it. An unknown type answers `pad.unknown-type` with the types
`config.conf` has.

## remove

`remove` takes away a set pad at your feet or under you, and answers `pad.removed`, or `pad.none-here` when there is
none. A matched pad cannot be removed this way: it is a pad because of its blocks, so change the blocks or the type.

## list

```
Pads set on blocks: 2
- classic at world 12 64 -3, 180 degrees
- bounce at world -40 70 18, 0 degrees
```

Related: [Set pads](../pads/set-pads.md), [Overview](overview.md), [Permissions](../permissions/reference.md)
