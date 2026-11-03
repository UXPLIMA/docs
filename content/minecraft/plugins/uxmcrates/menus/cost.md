---
title: Cost
order: 407
description: Every line a crate takes before it opens, and whether the player can pay it.
icon: coins
---

`/crates cost <crate>`. Three rows.

## Slots

| Slot | Tile | Click |
|---|---|---|
| 10 to 16 | One cost line each, a gold nugget | Nothing |
| 22 | Feather, `Back` | The [crate list](crates.md) |
| Every other slot | Grey pane | Nothing |

## A line

| Line | From |
|---|---|
| Title | The cost line as the player reads it, in `money` colour |
| Crumb | `Cost` |
| Description | `menu.cost.description` |
| State | `You can pay this.` or `You cannot pay this.` |

The key itself is not a cost line: the [crate list](crates.md) shows the keys held.

Related: [Crates](../features/crates.md#permission-requirements-and-cost)
