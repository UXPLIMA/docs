---
title: Concepts
order: 103
description: Arenas, the course generator, what a player chooses, the life of a run, and the boards.
icon: lightbulb
---

Everything in uxmParkour is one of five ideas. This page explains each in a few lines and links to the page with
the details.

## Arenas

An arena is four pieces in one world, written in [`arenas.conf`](../config/arenas-conf.md):

| Piece | What it is |
|---|---|
| `start` | The block the first jump is made from. It must be inside the box. |
| `heading` | `north`, `east`, `south` or `west`: the way the course runs. Every jump is measured from it. |
| `corner1`, `corner2` | Two opposite corners of the box the course may never leave. The order does not matter. |
| `difficulty`, `theme` | What a player gets in this arena when they have chosen none. New arenas take `run.default-difficulty` and `run.default-theme`. |

A new arena is empty and shut. It opens only when every piece is set and the start block is inside the box; an
edit that breaks either shuts it again on its own. See [Arenas](../arenas/overview.md).

## The course

A run builds its course as it goes. When the player lands on the next block, the generator draws a jump the
difficulty allows and places the block, so `run.blocks-ahead` blocks always stand in front, and removes the blocks
more than `run.blocks-behind` behind.

| Rule | Detail |
|---|---|
| Possible jumps only | Four blocks flat, three when rising one, more as the landing drops, and never a rise of two. A difficulty that leaves no possible jump is refused on reload. |
| Inside the box | A landing, and the two blocks of air a player needs above it, must be inside the box. |
| No doubling back | A landing may not take a block, or the air above one, that is still standing in this run. |
| Turning | When the way ahead runs out, at the wall of the box or into its own blocks, the course turns: straight on first, back the way it came last. |
| Exhaustion | When no jump fits anywhere, the run ends with the `exhausted` moment. A bigger box makes this rarer. |
| Seed | Every run has a seed; the same seed in the same arena gives the same course. `run.announce-seed` tells the player theirs. |

Every course of every player is generated alone. Two players in one arena do not see each other's course as taken,
so a busy arena needs room. See [How the course is built](../arenas/overview.md#how-the-course-is-built).

## What a player chooses

A player chooses a theme with `/parkour theme` or the [theme picker](../menus/themes.md), and a difficulty with
`/parkour difficulty`. The choice is kept in the database and used in every arena. For each run:

| Order | Difficulty | Theme |
|---|---|---|
| 1 | The player's own choice, when it still exists | The player's own choice, when it still exists |
| 2 | The arena's `difficulty` | The arena's `theme` |
| 3 | `run.default-difficulty` | `run.default-theme` |

A choice whose difficulty or theme was removed from `config.conf` is skipped, not refused.

## The life of a run

| Stage | What happens |
|---|---|
| Join | `/parkour` or `/parkour join [arena]`. The player's inventory is taken and held, the restart and leave items are put in, the first blocks are placed and the player is moved onto the start block. |
| Running | No damage, no dropped items, no clicks in the inventory, no hand swap. A right click with a session item restarts or leaves. |
| Milestone | On the jump count of a milestone, its reward is paid and the `milestone` moment plays. |
| End | A fall, `/parkour leave`, the leave item, a disconnect, `/parkour restart`, the arena running out of room, or the server stopping. The blocks are removed, the inventory comes back, and the player is moved back to where they joined from. |
| Record | The jumps go on the player's record and on the boards. A run of zero jumps counts on no board. |

See [Runs](../arenas/runs.md) for each ending and what is said.

## Boards

Three boards, each kept for every period in `leaderboard.periods`:

| Board | Counts | A run adds |
|---|---|---|
| `best` | The longest run | Its jumps, when higher than the player's best in that period |
| `jumps` | Every jump | Its jumps |
| `runs` | Runs | One |

The periods are `alltime`, `monthly`, `weekly` and `daily`, each starting again on its own in `leaderboard.time-zone`.
The boards are read from the database every `leaderboard.refresh-seconds` and answered from memory, so a hologram
costs nothing. See [Placeholders](../placeholders/reference.md#boards).
