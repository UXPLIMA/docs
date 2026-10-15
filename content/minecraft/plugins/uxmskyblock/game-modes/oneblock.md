---
title: OneBlock
order: 20
description: An island that starts as one block. Break it and it comes back as something new,
  drawn from the phase the island has reached.
icon: box
---

A OneBlock island is a single block in the void. Every break brings it back as another block,
drawn from the island's current phase, and sometimes a creature with it. A phase lasts a number of
breaks, counted over the whole life of the island, and then the next one begins.

## Turning it on

1. Set `enabled = true` in `modules/oneblock.conf`.
2. Add `oneblock` to `presets.modes` in `modules/presets.conf`.

The shipped preset `oneblock` starts with `"uxm:oneblock"`, the action that sets the island's
block. Any preset whose start list names it plays OneBlock.

## The phases

`modules/oneblock.conf` lists the phases in the order an island goes through them:

```hocon
phases = ["plains", "underground", "snow", "ocean", "jungle", "nether", "end"]
```

Each phase is a file of its own, `oneblock/phases/<name>.conf`. A phase's name is also the key its
title is read by in the language files, under `oneblock.phases`.

| Setting | What it does |
| --- | --- |
| `icon` | The item the phases window shows for the phase |
| `preview` | What the window says the phase holds, in your words or a `@key` |
| `biome` and `biome-area` | The biome the phase gives the island, over the whole island or so many blocks from the block |
| `length` | How many breaks the phase lasts |
| `wait` | How long the block stays bedrock with a countdown when the phase begins, as `"30s"` |
| `next` | The phases that may follow. Two or more make a branch the island picks once |
| `requires` | What must hold before the phase begins, such as `have level 25` or `take money 5000` |
| `blocks` | What the block turns into, each with a weight |
| `creature-chance` and `creatures` | How often a creature comes, and which |
| `loot` | The loot table a chest, barrel or shulker box drawn in the phase is filled from |
| `moments` | What happens at exact breaks: a block, a creature, loot, actions or a line over the block |
| `began`, `finished`, `first-finished` | Effect lists run when the phase begins and ends |
| `lucky` | A chance per break to run a list of rewards |

A block may carry its states, as `"oak_stairs[facing=north]" = 2`. An entry written as an object
places a spawner of a chosen creature. Blocks of ItemsAdder, Nexo or CraftEngine and creatures of
MythicMobs are written with the plugin's word in front, as `"nexo:amber" = 2`, and read while
that plugin is on the server.

A requirement is had or taken, from the breaker or from the island bank, and only a member
allowed to spend from the bank pays out of it. Until it holds, the island waits at the end of the
phase before, and each break tells the breaker what is missing.

## The rest of the file

| Setting | Shipped | What it does |
| --- | --- | --- |
| `after-last-phase` | `repeat` | `stay` keeps the island in the last phase, `repeat` starts again from the first |
| `save-interval` | `30s` | How often the breaks counted in memory are stored. A crash loses at most this much |
| `choose-within` | `60s` | How long an island at a branch has to pick its way before the first one is taken |
| `progress.bar` | `bossbar` | Where progress shows until a player switches it: `bossbar`, `actionbar` or `none` |
| `progress.linger` | `10s` | How long the boss bar stays after the last break |
| `progress.line` | `true` | Whether a line over the block shows the phase and how far into it the island is |
| `creature-warning` | `5` | How many breaks before a hostile creature the island is told. `0` tells nothing |

The blocks and creatures are drawn that many breaks ahead, so a warning is always right.

`/is reload` reads this file and every phase file again, and each island goes on where it stands.
`enabled` and `save-interval` change at the next restart.

## Windows

- `menus/island-oneblock.conf`: where the island stands, opened with `/is oneblock`.
- `menus/oneblock-branch.conf`: the ways a branching phase offers.
- `menus/oneblock-phases.conf`: every phase with its icon and preview.

A Bedrock player gets each of them as a form, and reads the line over the block on the action
bar.

## Commands

| Command | What it does |
| --- | --- |
| `/is oneblock` | Shows where your OneBlock island stands |
| `/isadmin oneblock setphase <target> <phase>` | Puts an island in a phase |
| `/isadmin oneblock setcount <target> <count>` | Sets an island's count of breaks |
| `/isadmin oneblock addcount <target> <count>` | Adds to it, or takes away with a negative number |
| `/isadmin oneblock check <target>` | Says where an island stands |
| `/isadmin oneblock respawn <target>` | Lays the island's block again |

`<target>` is a player's name or an island's id. A change of count lays the block again, drawn
from the phase the island stands in now. Each verb asks `uxmskyblock.admin.oneblock.<verb>`.

## Effects

`modules/effects.conf` holds the lists a phase falls back to when its own file writes none:
`oneblock-phase-began`, `oneblock-phase-finished`, `oneblock-lucky-break`, `oneblock-moment` and
`oneblock-creature-coming`. Each is a list of messages, titles, sounds and commands, as long as you
like.
