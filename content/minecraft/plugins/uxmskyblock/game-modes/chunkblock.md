---
title: ChunkBlock
order: 303
description: One chunk with a magic block, which opens the chunks beside it.
icon: grid-2x2
---

A ChunkBlock island starts as the one chunk its magic block stands in. The block comes back as something else each
time it is broken, as a OneBlock island's does, and draws from the phases of [OneBlock](oneblock.md). The island opens
the chunks beside it, each at the price of the grade it falls into.

## Opening chunks

A chunk opens only beside the chunks already open, never across a corner, and never past the island's edge.

| Kind | How it opens | What happens when the level falls |
|---|---|---|
| Held | Its price is the island level alone; the unlock ladder sets the level the next held chunk needs | The newest held chunks close first, and players in one are moved to the island spawn. Nothing built there is removed |
| Bought | Its price takes anything else: money, chunk points, items | It stays open |

The ladder ships as level 1 for the first chunk, 3 for the second, then 6, 10 and 15, then five more for each one
after. The grades (common, rare, far, epic) and their prices are in [chunkblock.conf](../config/chunkblock-conf.md).
A sign stands over each closed chunk the island can open next.

## Playing

| Command | What it does |
|---|---|
| `/is chunks` | Opens the [chunks window](../menus/game-modes.md#chunks) |

Look at a closed chunk from the edge of your island and click **Open the chunk you face** to see its price; the
[chunk window](../menus/game-modes.md#chunk) pays each line from what you carry or the island bank.

## Staff

`/isadmin chunkblock open`, `close` and `reset`; see [Staff](../commands/admin.md#isadmin). Placeholders:
`%uxmskyblock_chunkblock_open_chunks%` and `%uxmskyblock_chunkblock_next_level%`.
