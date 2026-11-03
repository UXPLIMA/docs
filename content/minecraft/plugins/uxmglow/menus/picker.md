---
title: Picker
order: 401
description: "The /glow window: sixteen dyed squares, their states, and the off button."
icon: palette
---

`/glow` opens five rows: the sixteen colours and a button to stop glowing.

## Slots

| Slot | Tile | Click |
|---|---|---|
| 10 to 16, 19 to 25, 30, 32 | One colour each, a leather chestplate dyed in it | A free colour: glow in it. A locked one: refused with a bass note |
| 39 | Arrow | The previous page |
| 40 | Barrier, `Turn the glow off` | Stops glowing |
| 41 | Arrow | The next page |
| Every other slot | Grey pane | Nothing |

The sixteen squares fit on one page, so the arrows do nothing as shipped. The worn colour glows.

## A colour

The tile reads the block of its state, `menu.colour.<state>`:

| State | Title colour | State line | Action line |
|---|---|---|---|
| `free` | `value` | `Ready` | `Click to glow in this colour` |
| `worn` | `good` | `Worn now` | `This is the colour you glow in` |
| `locked` | `muted` | `Locked` | `You do not have this colour yet` |

Crumb `Glow colour`, description `Your outline glows in this colour. Everybody near you sees it.` The title is the
colour's name.

## The off button

| State | Glow line | Action line |
|---|---|---|
| `on` | `On` | `Click to stop glowing` |
| `off` | `Off` | `You are not glowing` |

## Dyed squares

Every colour ships as `tinted:LEATHER_CHESTPLATE`, dyed with the exact colour the player will glow in. Leather is the
only vanilla item that takes an exact value, so it is the only material that shows sixteen colours as sixteen
different squares. Name another material and you get it untinted.

The picker sets solid colours; a cycle is started with `/glow cycle`.

Related: [Menu files](../config/menu-files.md), [Colours and cycles](../features/colours-and-cycles.md)
