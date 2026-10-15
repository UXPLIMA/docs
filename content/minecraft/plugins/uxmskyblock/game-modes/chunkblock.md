---
title: ChunkBlock
order: 30
description: An island that starts as the one chunk its magic block stands in, and opens the
  chunks beside it by level, by payment, or both.
icon: grid-3x3
---

A ChunkBlock island starts as one chunk with a magic block in it. The block comes back as
something else each time it is broken, the way a [OneBlock](oneblock.md) island's does, and draws
from the phases of `modules/oneblock.conf`. The island opens the chunks beside it one at a time.

- A chunk opens only beside the chunks already open, never across a corner, and never past the
  island's edge.
- Players cannot walk or build in a closed chunk.
- Nothing built in a chunk that closes is removed. It waits there.

## Turning it on

1. Set `enabled = true` in `modules/chunkblock.conf`, and in `modules/oneblock.conf`, whose
   phases the magic block draws from.
2. Add `chunkblock` to `presets.modes` in `modules/presets.conf`.

The shipped preset `chunkblock` starts with `"uxm:oneblock"` and `"uxm:chunkblock"`: the magic
block, then the one chunk around it. Its Nether and End each start as one chunk on a platform, a
web of their own: their chunks open the way the overworld's do, priced by the same grades. Take a
dimension out of the preset to keep a ChunkBlock island out of it.

## Held by the level, or bought

A chunk priced by the level alone is held, not bought. The island keeps it while its level stays
at or above what the chunk needed. When the level falls, the newest such chunks close first, and
players standing in one are moved to the island's spawn. A chunk whose price took anything else
was bought, and stays open for good.

| Setting | Shipped | What it does |
| --- | --- | --- |
| `unlock-levels` | `[1, 3, 6, 10, 15]` | The level the first chunks beyond the starting one need, one by one |
| `then-every` | `5` | How many more levels each chunk after that list needs than the one before |
| `keep-level-chunks` | `false` | On, an island never loses ground it reached, and only the next chunk waits for the level |
| `signs` | `true` | A sign floats over each closed chunk the island can open next, naming its grade |
| `bypass-permission` | `""` | A node whose holder walks and builds in closed chunks, such as staff. Empty lets nobody through |

## Grades

Each chunk of each island falls into one grade, drawn once from the grades of its ring and kept
for good, so a chunk shows every member the same price after every restart. The ring is how far
the chunk lies from the first: the eight around it are ring 1, the sixteen around those ring 2.

```hocon
tiers {
    common {
        rings = "1-2"
        weight = 3
        cost = [ "level ladder", "item cobblestone 128", "item oak_log 32" ]
        bypass = [ "currency points 25 player" ]
        rewards = [ "[sound] entity.player.levelup 1.0 1.2" ]
        points = 1
    }
}
```

| Key | What it does |
| --- | --- |
| `name` | What a player reads. Left out, `@chunkblock.tiers.<grade>` |
| `rings` | The rings it falls on: `"1-2"`, `"3"`, or `"6+"` for six and every ring past it |
| `weight` | How often it falls against the other grades of the same ring |
| `cost` | Every line opening a chunk of it takes. Members pay a little at a time, and what one hands in counts for all |
| `bypass` | Lines that open a chunk of it at once instead, all of them together |
| `rewards` | Effect lines fired for the player who opens it |
| `points` | The chunk points opening one gives the island |

A cost or bypass line is one of:

| Line | What it asks |
| --- | --- |
| `money <amount> [player\|island]` | The economy's money, from the player or the island bank |
| `currency <id> <amount> [player\|island]` | A currency of `modules/bank.conf` |
| `item <item> <amount>` | Items handed in from the inventory |
| `points <amount>` | The island's chunk points |
| `level <level>` | An island level, held and not spent. `level ladder` asks the ladder's level |

With no grade written, every chunk is priced by the level ladder alone. A line the file cannot
read is logged and left out, and so is a grade with no cost left.

## Windows

- `menus/island-chunks.conf`: how far the island has opened, from `/is chunks`.
- `menus/island-chunk.conf`: one closed chunk, its grade, every line of its price and the buttons
  that pay for it.

A member who clicks a chunk's sign opens its window. A Bedrock player gets each as a form.

## Commands

| Command | What it does |
| --- | --- |
| `/is chunks` | Shows how far your ChunkBlock island has opened |
| `/isadmin chunkblock open <target> [chunk_x chunk_z]` | Opens a chunk, the one you stand in or the one named |
| `/isadmin chunkblock close <target> [chunk_x chunk_z]` | Closes a chunk |
| `/isadmin chunkblock reset <target>` | Closes every chunk but the first |

A chunk an operator opens stays open whatever the level does, as one paid for does. Whoever stands
in a chunk that closes is moved, as when the level closes it. Each verb asks
`uxmskyblock.admin.chunkblock.<verb>`.
