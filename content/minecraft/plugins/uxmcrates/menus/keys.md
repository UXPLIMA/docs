---
title: Keys
order: 504
description: Every key the player holds, the crates each opens, and where to get one.
icon: key-round
---

`/crates keys`, or the tripwire hook in the crate list.

## Slots

| Slot | Tile | Click |
|---|---|---|
| 10 to 16, 19 to 25, 28 to 34 | One key each, by the key's `menu.order` | Left and right run the key's `menu.click.left` and `click.right` |
| 48 | Arrow | The previous page |
| 49 | Feather, `Back` | The [crate list](crates.md) |
| 50 | Arrow | The next page |
| Every other slot | Grey pane | Nothing |

A key with `menu.hidden = true` is not shown.

## A key

| Line | From |
|---|---|
| Title | The key's name |
| Crumb | `Key` |
| Description | `menu.key.description` |
| You hold | Items and balance together |
| Opens | Every crate that takes this key |
| Store lines | The key's `menu.store`, only while the player holds none |

The item is the key's own item. A click does nothing unless the key file gives it lines, such as
`click { left = ["[player] shop keys"] }`.

Related: [Keys](../features/keys.md), [Key files](../config/key-files.md#menu)
