---
title: Arenas
order: 301
description: The box, the start block and the heading, how the course is built inside them, and an arena's state.
icon: box
---

An arena is where runs happen: a box in one world that no course may leave, a start block inside it, and the
heading the course sets off in. It holds no blocks of its own. Every block a player jumps on is placed by their run
and removed by it.

## The pieces

| Piece | Written as | What it decides |
|---|---|---|
| World | `world` | The world the box is in. A box spans one world. |
| Start block | `start` | Where the first jump is made from. The player is moved onto it when a run begins. It must be inside the box. |
| Heading | `heading` | `NORTH`, `EAST`, `SOUTH` or `WEST`. The first jumps go that way, and every jump is measured in that frame. |
| Corners | `corner-one`, `corner-two` | Two opposite corners of the box, in any order. Both blocks are inside it. |
| Difficulty | `difficulty` | The difficulty of a player who has chosen none. |
| Theme | `theme` | The theme of a player who has chosen none. |
| Open | `enabled` | Whether players may join. |

The pieces are set with `/uxmparkour set`, from the [editor](../menus/editor.md), or by hand in
[`arenas.conf`](../config/arenas-conf.md).

## The arena's state

| State | Means | Shown in the list as |
|---|---|---|
| Incomplete | A piece is missing: start, heading, a corner | `(incomplete)`, a barrier |
| Shut | Every piece is set, and players may not join | `(shut)`, a grey dye |
| Open | Every piece is set, the start is inside the box, and players may join | `(open)`, a lime dye |

- A new arena is incomplete and shut. `enable` refuses until every piece is set, then refuses again while the
  start block is outside the box.
- An open arena that an edit makes unplayable shuts itself: a corner moved so the start falls outside, or a
  change of world, which drops every position. The reply says why.
- A shut arena cannot be joined by name or picked by a bare `/parkour`; it answers as though it did not exist.
- A run in progress is never moved by an edit. A changed box applies to the next run.

## How the course is built

The generator is plain arithmetic on block positions and one random sequence, seeded per run.

1. The difficulty has already been reduced to the list of jumps a player can make: see
   [Difficulties](difficulties.md).
2. For the next block, the generator draws jumps from that list at random, without drawing the same one twice,
   until one fits.
3. A jump fits when its landing, and the two blocks of air above it, are inside the box, and when neither the
   landing nor that air is a block of this run still standing.
4. When no jump fits in the current heading, the heading turns: straight on first, then to the sides, and back the
   way it came last. The course goes as straight as the box allows and turns only when it must.
5. When no jump fits in any heading, the course is exhausted. The player finishes the blocks already in front of
   them, then the run ends with the `exhausted` moment.

Only the blocks still in the world count as taken. A block removed behind the player frees its space, so a long
run can cross ground it used before. The material of each block is drawn from the theme by the same random
sequence, so the same seed in the same arena, difficulty and theme gives the same course on every server.

## Sizing the box

| Box | Behaviour |
|---|---|
| Long and narrow, along the heading | Runs straight for a long time, then turns and comes back |
| Wide and flat | Turns often; a `hard` difficulty with a sideways reach of 3 uses the width |
| Tall | Leaves room for the course to climb and drop; `min-rise` and `max-rise` set how much it does |
| Too small for the difficulty | Runs end early as exhausted |

A run that keeps ending as exhausted is the sign of a box too small for its difficulty. The start block sits best a
few blocks in from a short side, with the heading along the long side.

<Callout type="danger" title="Blocks inside the box are overwritten">

The course does not look at the world. A block it places takes the place of whatever was there, and it is set to
air when the player has passed, without physics, so nothing falls or breaks around it. Anything inside the box,
your map included, can be replaced and then deleted. Keep the box in empty air.

</Callout>

## Several players in one arena

Any number of players may run one arena at once. Each run builds its own course and does not see the others'
blocks as taken, so two courses can cross. Give a busy arena a box large enough that courses rarely meet, or build
one arena per few players: a bare `/parkour` takes everyone into the first open arena, and `/parkour join <arena>`
spreads them out.

## Notes

- **The seed.** `run.announce-seed = true` tells a player the seed of every run, which is useful only to compare
  two runs.
- **The world must be loaded.** An arena in an unloaded world cannot be joined, and `/uxmparkour tp` answers that
  the world is missing.
- **Arenas are files, not database rows.** On a network, copy `arenas.conf` to every server that runs the same
  arena. The records and boards are what the database shares.

Related: [Your first arena](../getting-started/first-arena.md), [arenas.conf](../config/arenas-conf.md),
[Arena editor](../menus/editor.md)
