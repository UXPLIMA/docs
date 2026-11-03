---
title: Milestones
order: 506
description: The rungs of a milestone ladder, the player's progress, and taking a rung.
icon: flag
---

`/crates milestones <crate>` shows the per-crate ladder counted for that crate; `/crates milestones` the ladder that
counts every crate.

## Slots

| Slot | Tile | Click |
|---|---|---|
| 10 to 16, 19 to 25, 28 to 34 | One rung each, an experience bottle whose stack size is the rung's count | Left: take it |
| 48 | Arrow | The previous page |
| 49 | Feather, `Back` | The [crate list](crates.md) |
| 50 | Arrow | The next page |
| Every other slot | Grey pane | Nothing |

## A rung

| Line | From |
|---|---|
| Title | The rung's `name` |
| Crumb | `Milestone` |
| Description | `menu.milestone.description` |
| Openings | `openings / rung`, such as `7 / 10` |
| Counts | The crate's name, or `Every crate` |
| Action | `Click to take it.` when reached, `Keep opening.` before, `You took this one.` after |

A click before the rung is reached answers `milestone.not-ready`. A rung with `automatic = true` was handed over when
it was reached and shows as taken.

Related: [Rerolls and milestones](../features/rerolls-and-milestones.md#milestones)
