---
title: vanilla.conf
order: 605
description: Vanilla enchantments drawn in our block, with their names, descriptions and type.
icon: book
---

The display hides the client's own enchantment lines, so the vanilla enchantments are drawn in our block, after ours,
in the same formats. This whole file is optional, and so is every key.

| Key | Meaning |
|---|---|
| `name` | What a player reads; left out, the key read as words: `minecraft:fire_aspect` is "Fire Aspect" |
| `description` | What it does; left out, none is drawn |
| `type` | The type whose format it is drawn in; left out, the first type |

The namespace is optional: `sharpness` and `minecraft:sharpness` are the same key.

## Shipped

| Key | Name | Description |
|---|---|---|
| `sharpness` | Sharpness | Cuts deeper. |
| `protection` | Protection | Turns aside a part of every blow. |
| `efficiency` | Efficiency | Breaks blocks faster. |
| `unbreaking` | Unbreaking | Wears out more slowly. |
| `mending` | Mending | Repairs itself from the experience you collect. |
| `fortune` | Fortune | Yields more from what you mine. |
| `looting` | Looting | Yields more from what you kill. |
| `silk_touch` | Silk Touch | Takes the block itself. |
| `fire_aspect` | Fire Aspect | Sets what you hit alight. |
| `power` | Power | Draws a heavier arrow. |

Related: [Display](../features/display.md#vanilla-enchantments)
