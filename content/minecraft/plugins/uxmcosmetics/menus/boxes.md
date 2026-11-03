---
title: Mystery boxes
order: 504
description: Every box with its price, and opening one.
icon: package-open
---

`/cosmetics boxes`, or a station of every box. Three rows.

## Slots

| Slot | Tile | Click |
|---|---|---|
| 10 to 16 | One box each, its `icon`, in file order | Opens it: the [roulette](roulette.md) |
| 21 | Arrow | The previous page |
| 22 | Feather, `Back` | The wardrobe |
| 23 | Arrow | The next page |
| Every other slot | Grey pane | Nothing |

## A box

| Line | From |
|---|---|
| Title | The box's name, in its tier's colour from `menu.colours.tiers` |
| Crumb | `Mystery box` |
| Description | `menu.boxes.box.description` |
| Price | In `economy.format` |
| Action | `Click to open it` |

Every box is shown, whether the player may open it or not; a refused click answers `box.not-permitted`.

Related: [Mystery boxes](../features/mystery-boxes.md)
