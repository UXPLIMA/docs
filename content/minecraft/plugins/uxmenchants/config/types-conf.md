---
title: types.conf
order: 503
description: The two shipped types, every key of a type, and the format tokens.
icon: type
---

| Key | Meaning |
|---|---|
| `name` | What a player reads for the type |
| `order` | Where it sorts in a lore block and a window; lowest first |
| `format` | The line an enchantment of this type is drawn as |
| `limit-per-item` | How many of this type one item may carry; `-1` for no limit |
| `grindstone-removable` | Whether a grindstone takes it off |

| Token | Value |
|---|---|
| `<name>` | The enchantment's name, in the reader's language |
| `<roman>` | The level in roman numerals |
| `<number>` | The level as digits |

Do not write `<level>`: it is a colour role and the palette eats it. Colours are roles of `theme.conf`.

## Shipped

| Type | Reads | `order` | `format` | `limit-per-item` | `grindstone-removable` |
|---|---|---|---|---|---|
| `normal` | Enchantment | 1 | `<value><name> <roman>` | -1 | true |
| `curse` | Curse | 2 | `<bad><name> <roman>` | 2 | false |

Related: [Grindstone, types and targets](../features/grindstone-types-targets.md)
