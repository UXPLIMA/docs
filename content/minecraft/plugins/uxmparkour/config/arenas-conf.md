---
title: arenas.conf
order: 502
description: The arenas, as the admin commands and the editor write them, and editing them by hand.
icon: map
---

`arenas.conf` holds every arena. The admin commands and the editor write it; you may edit it by hand too.

```hocon
arenas {
  sky {
    world      = "world"
    heading    = "EAST"
    difficulty = "normal"
    theme      = "quartz"
    enabled    = true
    start      { x = 0,   y = 120, z = 0 }
    corner-one { x = -40, y = 110, z = -40 }
    corner-two { x = 160, y = 170, z = 40 }
  }
}
```

## The keys

| Key | What it holds |
|---|---|
| the block name | The arena's id: lower-case letters, digits, `_` and `-`, up to 32 characters |
| `world` | The world, by name |
| `heading` | `NORTH`, `EAST`, `SOUTH` or `WEST` |
| `difficulty` | A difficulty id from `config.conf` |
| `theme` | A theme id from `config.conf` |
| `enabled` | `true` when players may join |
| `start` | The start block, `{ x, y, z }` |
| `corner-one`, `corner-two` | Two opposite corners of the box, in any order |

An arena is written the moment it is created, before any piece is set. A piece that is not set is absent, never
written as zero.

## Editing by hand

- A hand edit is read at a restart, or when any arena is next saved by a command or the editor, because a save reads
  the file before it writes. `/uxmparkour reload` does not read this file.
- An arena that cannot be read is skipped with its name in the console, and the others are still loaded.
- An arena written without `enabled` counts as open, so an arena from an older file stays playable. Write
  `enabled = false` to keep one shut.
- An arena whose start block is outside its box cannot be joined whatever `enabled` says; `/uxmparkour show` names the
  reason.
- Turning an arena around needs only `heading`.

## On a network

Arenas are files, not database rows. Copy `arenas.conf` to every server that runs the same arenas; the records and
the boards are what a shared database shares.

Related: [Arenas](../arenas/overview.md), [Operator commands](../commands/admin.md), [Arena editor](../menus/editor.md)
