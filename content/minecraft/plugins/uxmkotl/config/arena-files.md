---
title: Arena files
order: 502
description: The file of one arena under arenas/, every key, what a missing key means, and editing by hand.
icon: map
---

Every arena is one file, `arenas/<id>.conf`, where the file name is the arena's id. The
[operator commands](../commands/admin.md) and the [editor](../menus/editor.md) write it; you can also write or copy
one by hand. To copy an arena to another server, copy its file.

```hocon
boundary {
  x1 = 18
  y1 = 61
  z1 = -48
  x2 = 28
  y2 = 76
  z2 = -38
}
display-name = "hill"
enabled = true
entry {
  clear-effects = true
  clear-inventory = true
  game-mode = ADVENTURE
}
exit {
  world = world
  x = 23.5
  y = 62.0
  z = -32.5
  yaw = 0
  pitch = 0
}
join-on-enter = true
plate {
  x = 23
  y = 68
  z = -43
}
spawn {
  world = world
  x = 23.5
  y = 62.0
  z = -38.5
  yaw = 180
  pitch = 0
}
world = world
```

## The keys

| Key | What it is | Left out |
|---|---|---|
| `world` | The world of the box, the plate and the spawn | The arena has no world and is incomplete |
| `display-name` | The name players read in messages | The id |
| `enabled` | Whether players can get in | `true` |
| `join-on-enter` | Whether walking into the box joins | `true` |
| `boundary.x1`, `y1`, `z1` | Corner one, a block | Corner one is not set |
| `boundary.x2`, `y2`, `z2` | Corner two, a block | Corner two is not set |
| `plate.x`, `y`, `z` | The plate, a block | Not set |
| `spawn { world, x, y, z, yaw, pitch }` | Where a joining or respawning player is put. `world` defaults to the arena's | Not set |
| `exit { world, x, y, z, yaw, pitch }` | Where a leaving player is put. `world` defaults to the arena's | Not set |
| `entry.clear-inventory` | Put the items away on entry, give them back on leaving | `true` |
| `entry.clear-effects` | Remove potion effects on entry | `true` |
| `entry.game-mode` | `SURVIVAL`, `CREATIVE`, `ADVENTURE` or `SPECTATOR` | `ADVENTURE` |

A block piece is three whole numbers; a place takes decimals, and `x = 23.5` is the middle of block 23. `yaw` is the
direction faced: `0` south, `90` west, `180` north, `-90` east. `pitch` is up and down, `0` level.

## Rules

- **The file name is the id.** It must be a valid id: one to 32 lower-case letters, digits and `-`, starting with a
  letter or a digit. A file whose name is not is skipped with a warning.
- **The plate must be inside the box.** An arena whose plate is outside is incomplete, and the console says so for one
  that is switched on.
- **A file that cannot be read is skipped**, with the reason in the console, and the other arenas load.
- **Missing is not zero.** A piece left out is not set; it is never read as `0 0 0`.

## Editing by hand

Edit the file and run `/uxmkotl reload`: the arena files are read again with the rest. A file you add is a new arena,
and a file you delete removes it. An edit by command or editor writes the whole file again from what the plugin
holds, so your comments in an arena file do not survive the next edit.

Related: [Arenas](../arenas/overview.md), [Operator commands](../commands/admin.md), [Arena editor](../menus/editor.md)
