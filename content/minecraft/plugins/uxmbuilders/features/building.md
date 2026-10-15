---
title: Building and performance
order: 201
description: How blocks are placed, in what order, and how the server stays at 20 TPS.
icon: gauge
---

One task runs every tick and places blocks for every building job. Each job advances a cursor along
a precomputed order of the schematic's blocks, so a 200,000-block castle is an array of indices, not
200,000 objects.

## Speed

A job's speed is its builder's `blocksPerSecond` at the level it started with, then adjusted:

| Step | Effect |
|---|---|
| Package affinity | `builderAffinity` in the package multiplies it for that builder |
| `uxmbuilders.speed.<n>` | Adds `n` percent; the highest node held wins |
| Speed boost | Multiplies it while a [boost](boost.md) runs |
| Load factor | `1.0` normally, `slowdownFactor` under load, `0` below `pauseBelowTps` |
| Server ceiling | Never more than `performance.blocksPerTick` blocks per tick |

Fractions carry over between ticks: a builder at 2 blocks per second places one block every ten ticks
rather than none.

## The shared budget

Every building job draws from the same two limits each tick, and the jobs take turns starting first so
none of them starves.

| Key | Default | Limit |
|---|---|---|
| `performance.blocksPerTick` | `20` | Blocks placed per tick, server-wide |
| `performance.maxNanosPerTick` | `2000000` | Time spent placing per tick (2 ms) |
| `performance.maxConcurrentJobs` | `5` | Jobs building at once; the rest queue |
| `performance.queueLimit` | `20` | Jobs allowed to wait in the queue |

Paused jobs hold no slot and do not count toward the queue limit.

## Adaptive slowdown

`performance.adaptive` watches the average tick time of the last five seconds, not the one-minute TPS,
so a chunk-loading spike does not slow builds for minutes.

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `true` | Turn the whole mechanism off |
| `tpsThreshold` | `19.0` | Below this, speed is multiplied by `slowdownFactor` |
| `slowdownFactor` | `0.5` | The multiplier under load |
| `pauseBelowTps` | `17.0` | Below this, nothing is placed |
| `recoverAfterTicks` | `100` | How long a pause lasts after a drop |

`/builders admin perf` shows the current factor, blocks per second, tick time and every job.

## Build order

The order decides how the build looks while it goes up and how far the NPC walks between blocks.

| Value | Look |
|---|---|
| `GREEDY` | Each next block is the closest unplaced one; a natural walk |
| `LAYERED` | Bottom-up serpentine rows |
| `SPIRAL` | From the centre outwards |
| `NEAREST` | Closest to the origin first |
| `RANDOM` | Materialises piece by piece |

The order comes from the builder's `buildOrderOverride` first, then the package's `buildOrder`, then
`schematic.buildOrder`. An unknown value is logged with the valid ones and falls back.

With `schematic.twoPass: true`, blocks that need support (sand, carpets, doors, signs, torches, saplings) go
in a second pass, after what they stand on or hang from.

## What gets placed

Blocks go in directly, without neighbour updates or physics, which is what keeps the cost per block low.

| Rule | Detail |
|---|---|
| Already there | A block whose exact state already stands is skipped and costs nothing |
| Blacklist | `schematic.blacklist` blocks are never placed (spawners, command blocks, bedrock...) |
| Fluids | Water and lava are dropped unless `schematic.allowFluids` is on; waterlogged blocks keep the block |
| Unknown blocks | `schematic.unknownBlock`: `SKIP`, `REPLACE` with `unknownReplacement`, or `ABORT` |
| Signs | Text is kept with `keepSignText`, as plain text |
| Containers | A chest in the way has its contents dropped, never deleted |
| Size | Schematics over `schematic.maxBlocks` are refused before they are read |

A block the world keeps rejecting is retried three times, then skipped and logged.

## Chunks

With `performance.chunk.keepLoaded`, the chunks a job builds in stay loaded until it finishes or
pauses, up to `maxTickets` per job. Two neighbouring builds share a chunk safely: it is released when the
last of them leaves.

## Notes

- Progress milestones (`progress.chatMilestones`) are chat lines at 25, 50, 75 and 100 percent.
- A per-block `BuildBlockPlaceEvent` exists for other plugins but is off by default
  (`api.fireBlockEvents`); it has a measurable cost on large builds.

Related: [Builders](builders.md), [config.yml](../config/config-yml.md)
