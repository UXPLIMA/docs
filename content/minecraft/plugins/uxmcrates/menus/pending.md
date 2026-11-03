---
title: Waiting rewards
order: 405
description: Rewards that were decided and not yet delivered, and taking them.
icon: inbox
---

`/crates claim`, or the minecart in the crate list. A reward lands here when the inventory was full, when a reveal was
interrupted, while a reroll is undecided, or when a key item did not fit.

## Slots

| Slot | Tile | Click |
|---|---|---|
| 10 to 16, 19 to 25, 28 to 34 | One waiting reward each | Left: take it |
| 48 | Arrow | The previous page |
| 49 | Feather, `Back` | The [crate list](crates.md) |
| 50 | Arrow | The next page |
| Every other slot | Grey pane | Nothing |

## A reward

| Line | From |
|---|---|
| Title | The reward's name |
| Crumb | `Waiting` |
| Description | `menu.pending.description` |
| From | The crate, or `Not from a crate` for a key or an every-crate milestone |
| Won | How long ago it was decided |
| Action | `Click to take it.` |

The item is the first item of the reward, or paper for a reward that only runs commands or pays money.

| Click result | Message |
|---|---|
| Delivered | `pending.claimed` |
| No room, or only part fitted | `pending.no-room`; what did not fit stays |
| Already taken elsewhere | `pending.gone` |

A player with rewards waiting reads `pending.on-join` at every join.

Related: [Opening](../features/opening.md#pending-rewards)
