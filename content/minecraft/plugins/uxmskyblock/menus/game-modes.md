---
title: Game mode windows
order: 506
description: OneBlock, ChunkBlock, Brix, Parkour and TradeWinds windows.
icon: gamepad-2
---

## OneBlock

The file is `menus/island-oneblock.conf`. The window is titled "OneBlock" and has 3 rows.
The OneBlock window /is oneblock opens on a OneBlock island.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 10 | `GRASS_BLOCK` | The phase the block is in now. Each one brings blocks and mobs of its own. | opens the [phases window](#oneblock-phases) |
| 12 | `EXPERIENCE_BOTTLE` | This phase: How far through the phase your island is. |  |
| 16 | `OAK_SIGN` | Your progress: Where your island's progress shows while you break its block. | moves the progress bar |
| 14 | `IRON_PICKAXE` | Every break: Every block your island has broken since it began, and who broke them. |  |
| 22 | `FEATHER` | « Back | opens the [island menu window](island.md#island-menu) |

## OneBlock phases

The file is `menus/oneblock-phases.conf`. The window is titled "Every phase" and has 6 rows.
Every OneBlock phase, opened from the phase tile of the OneBlock panel: the phases the island has reached, the one it is in and the ones still locked to it. A Bedrock player gets this file as a form.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 4 | `CLOCK` | The phase your island is in now. The phases it has reached are lit, the ones ahead are locked. |  |
| 10-16, 19-25, 28-34 | `%entry_icon%` | One tile for each entry.  |  |
| 48 | `ARROW` | ← Previous page |  |
| 48 | `ARROW` | ← No page before this one |  |
| 49 | `FEATHER` | « Back | runs `/is oneblock` |
| 50 | `ARROW` | → Next page |  |
| 50 | `ARROW` | → No page after this one |  |

## OneBlock branch

The file is `menus/oneblock-branch.conf`. The window is titled "Pick the way on" and has 3 rows.
The ways on from a OneBlock phase that branches: the window the island's block opens at the end of such a phase, one tile for each phase it may go on to. A Bedrock player gets this file as a form.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 4 | `CLOCK` | ends: Your island goes on to one of these phases. Without a pick, the first is taken. |  |
| 11-15 | `GRASS_BLOCK` | One tile for each entry. A phase your island may go on to. Once picked, it is the way for good. | picks that way |

## Chunks

The file is `menus/island-chunks.conf`. The window is titled "ChunkBlock" and has 6 rows.
The ChunkBlock window /is chunks opens on a ChunkBlock island.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 2 | `GRASS_BLOCK` | Open chunks: Your island starts with one chunk and opens the ones beside it. |  |
| 4 | `EXPERIENCE_BOTTLE` | Next chunk: A higher island level opens the next chunk. |  |
| 6 | `FILLED_MAP` | Open the chunk you face: Stand at the edge of your island and look at the closed chunk. | closes the window, runs `/is chunks unlock` |
| 19-25, 28-34 | `MAP` | One tile for each entry. chunk: A closed chunk beside your open ground. | opens the chunk's price |
| 48 | `ARROW` | ← Previous page |  |
| 48 | `ARROW` | ← No page before this one |  |
| 50 | `ARROW` | → Next page |  |
| 50 | `ARROW` | → No page after this one |  |

## Chunk

The file is `menus/island-chunk.conf`. The window is titled "chunk" and has 6 rows.
One closed chunk of a ChunkBlock island: its grade, every line of its price, and the buttons that pay for it. The chunks window and the sign over a closed chunk open it.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 4 | `FILLED_MAP` | chunk: It opens once every line of its price is met. A chunk bought with anything but the level stays open for good. |  |
| 19-25, 28-34 | `%entry_material%` | One tile for each entry. . | pays that line |
| 48 | `ARROW` | ← Previous page |  |
| 48 | `ARROW` | ← No page before this one |  |
| 47 | `EMERALD` | Pay what you can: Hands in what you carry toward every line, and pays the island's lines from its bank when your role may. | pays what you can |
| 49 | `FEATHER` | « Back | runs `/is chunks` |
| 51 | `ENDER_PEARL` | Skip the price: Opens the chunk at once, for all of this together. | skips the price |
| 50 | `ARROW` | → Next page |  |
| 50 | `ARROW` | → No page after this one |  |

## Brix plots

The file is `menus/brix-gallery.conf`. The window is titled "Brix plots" and has 6 rows.
Brix: every plot on the server. The plots of the build contest under way come first, best first; the rest follow, the newest first.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 10-16, 19-25, 28-34, 37-43 | `BRICKS` | One tile for each entry. Built by 's team. | visits the island |
| 48 | `ARROW` | ← Previous page |  |
| 48 | `ARROW` | ← No page before this one |  |
| 50 | `ARROW` | → Next page |  |
| 50 | `ARROW` | → No page after this one |  |

## Parkour courses

The file is `menus/parkour-courses.conf`. The window is titled "Parkour courses" and has 6 rows.
Parkour: every course on the server, the most run first.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 10-16, 19-25, 28-34, 37-43 | `LIGHT_WEIGHTED_PRESSURE_PLATE` | One tile for each entry. On 's island. | visits the island |
| 48 | `ARROW` | ← Previous page |  |
| 48 | `ARROW` | ← No page before this one |  |
| 50 | `ARROW` | → Next page |  |
| 50 | `ARROW` | → No page after this one |  |

## Set sail

The file is `menus/tradewinds-sail.conf`. The window is titled "Set sail" and has 3 rows.
TradeWinds: the ports a vessel can sail for.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 10-16 | `%entry_material%` | One tile for each entry. A port your vessel can sail for. | `tradewinds:set-sail` |
| 21 | `ARROW` | ← Previous page |  |
| 21 | `ARROW` | ← No page before this one |  |
| 23 | `ARROW` | → Next page |  |
| 23 | `ARROW` | → No page after this one |  |

## Market

The file is `menus/tradewinds-market.conf`. The window is titled "Market of" and has 4 rows.
TradeWinds: the market of the port a vessel lies in.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 10-16, 19-25 | `%entry_material%` | One tile for each entry. A good this port trades, paid by the island bank. | Left: `tradewinds:buy`; Right: `tradewinds:sell` |
| 30 | `ARROW` | ← Previous page |  |
| 30 | `ARROW` | ← No page before this one |  |
| 32 | `ARROW` | → Next page |  |
| 32 | `ARROW` | → No page after this one |  |

## Contracts

The file is `menus/tradewinds-contracts.conf`. The window is titled "Contracts" and has 4 rows.
TradeWinds: the contracts the crew of a vessel holds, and those the port it lies in offers.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 10-16, 19-25 | `%entry_material%` | One tile for each entry.  | `tradewinds:contract` |
| 30 | `ARROW` | ← Previous page |  |
| 30 | `ARROW` | ← No page before this one |  |
| 32 | `ARROW` | → Next page |  |
| 32 | `ARROW` | → No page after this one |  |

## Cargo hold

The file is `menus/vessel-cargo.conf`. The window is titled "Cargo hold:" and has 6 rows.
The cargo hold of a TradeWinds vessel, opened with /is cargo by anyone of its team aboard it.
