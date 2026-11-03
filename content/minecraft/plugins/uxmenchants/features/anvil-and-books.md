---
title: Anvil and books
order: 407
description: Books, combining two items, the anvil cost, overpowering, recharging, and applying a book by clicking.
icon: anvil
---

## Books

A book carries an id and a level in its own data. It is drawn with `books.name-format` (`<accent><name> <roman>`), the
enchantment's description wrapped as on an item, and the `books.lore` lines under it (`@book.hint`: "Put this and an
item in an anvil."). It shimmers when `books.glow` is on. Its material is `item.book-material` (`ENCHANTED_BOOK`).

## The anvil

The server works out its own repair, rename and vanilla result first, and ours is added on top.

| Second slot | Result |
|---|---|
| A book of ours | The book's enchantment is applied under the [one rule](applying.md); a refusal leaves the item as it was |
| An item with ours | Both sets are summed under the same limit; two of the same enchantment at the same level make the next level, as vanilla does, and a level above the maximum is held at the maximum |
| `charges.fuel-material` | The item's charges are topped up by `charges.recharge`, when it carries ours |

`anvil.enabled` and `acquisition.anvil.enabled` must both be on.

## The cost

Vanilla says "Too Expensive!" at level forty, and one number solves it. Each enchantment of ours the anvil adds costs
`base-cost` levels times `cost-exponent` for each one added before it, on top of what the server charged for its own
work, and the total is held at 39 so the anvil never refuses. Raising a level adds nothing.

| `cost-exponent` | Feel |
|---|---|
| 1.0 | Flat: every enchantment costs `base-cost` |
| 1.1 | Gentle, the default |
| 1.5 | Steep: the fifth costs several times the first |
| 2.0 | Doubling: the vanilla feel without the level forty wall |

`vanilla-costs = true` keeps the server's own arithmetic and ignores both numbers.

## Overpowering

With `anvil.overpowering = true`, two of the same enchantment at its maximum may be pushed one level past it, up to the
enchantment's own `overpower-max`. An enchantment that writes none, or a number at or below `max-level`, cannot be pushed.
Only the anvil does this; a table that could would hand out a level no shop sold.

## Applying a book by clicking

```hocon
drag-and-drop {
  enabled   = false
  cost      = 5
  per-level = true
}
```

On, a left click with a book on the cursor onto a single item in the player's own inventory applies it, for `cost`
experience levels, times the book's level when `per-level` is on. A stack, a right click and a click in another
container are left alone. A refusal answers `drop.refused` and too few levels `drop.too-expensive`; neither takes the
book nor the levels. Success says `drop.applied`. It ships off, so a server that upgrades does not find its anvil
suddenly optional.

Related: [Applying](applying.md), [Charges and cooldowns](charges-and-cooldowns.md)
