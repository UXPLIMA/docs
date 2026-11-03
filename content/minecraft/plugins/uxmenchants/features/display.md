---
title: Display
order: 309
description: How enchantments are drawn on items, descriptions, collapsing, order, vanilla lines and struck-through lines.
icon: eye
---

The display never writes to the real item. A copy is built, decorated and shown, so renaming an enchantment changes
every item at once and nothing of ours can corrupt a lore another plugin is about to save.

```
Sharpened IV
  Cuts deeper with every level.
Vein Miner II
  Follows an ore through the stone.
Sharpness V
  Cuts deeper.
```

## The block

| Setting | Default | Meaning |
|---|---|---|
| `display.enabled` | true | Whether anything is drawn |
| `description-threshold` | 4 | Above this many of ours on one item, descriptions are dropped |
| `wrap-width` | 38 | Characters a description line holds before it wraps |
| `descriptions-on-by-default` | true | What a player who never used `/enchants descriptions` sees |
| `description-format` | `<subtext><line>` | One wrapped line of a description |
| `not-met-format` | `<dim><st><name> <roman></st>` | An enchantment this player cannot fire |
| `collapse.threshold` | 6 | Above this many, several names share a line |
| `collapse.per-line` | 3 | How many share it |
| `collapse.delimiter` | `", "` | What sits between them |
| `order` | `["type", "rarity", "name"]` | How the block is sorted; any of the three, in the order you want |

An enchantment is drawn in its type's `format`. Struck through answers "why did my enchantment not fire": it covers the
node, a fact that only changes when an operator changes it.

## Vanilla enchantments

The display hides the client's own enchantment lines so our block is the only one a player reads, and draws the vanilla
enchantments in the same block, after ours, in the same formats. Their names and descriptions come from
[vanilla.conf](../config/vanilla-conf.md); a key with no entry is drawn from the key itself, `minecraft:fire_aspect` as
"Fire Aspect".

## Glint and charges

`item.glint` makes an item with ours shimmer. With [charges](charges-and-cooldowns.md) on, a banded line says how many
are left.

## Language

Every name and description is drawn in the viewer's language. A reload redraws every player's items.

Related: [config.conf](../config/config-conf.md#display), [types.conf](../config/types-conf.md)
