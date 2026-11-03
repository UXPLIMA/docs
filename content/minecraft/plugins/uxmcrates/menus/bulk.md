---
title: Bulk summary
order: 508
description: "What one /crates massopen produced, counted by reward."
icon: layers
---

`/crates massopen <crate>` opens the keys without an animation and shows one summary.

## Slots

| Slot | Tile | Click |
|---|---|---|
| 10 to 16, 19 to 25, 28 to 34 | One reward each; the stack size is how many times it was won | Nothing |
| 48 | Arrow | The previous page |
| 49 | Feather, `Back` | The [crate list](crates.md) |
| 50 | Arrow | The next page |
| Every other slot | Grey pane | Nothing |

## A reward

| Line | From |
|---|---|
| Title | The reward's name |
| Crumb | `Summary` |
| Description | `menu.bulk.description` |
| Opened | How many openings the run made |
| How many | How often this reward came out |
| Note | `The operator caps a single call at <cap>.` when the cap stopped the run |

The rewards are already in the inventory or the waiting rewards; this window only counts them.

Related: [Opening](../features/opening.md#bulk-opening)
