---
title: Worn items
order: 408
description: Hats, suits and banners, the armour slots, colours, models, and banner patterns.
icon: hard-hat
---

Hats, suits and banners put marked items in the player's armour slots. The armour the player really wore is put back
when the piece comes off.

## Hats

`item = "GLASS"` puts one item on the head. Wear a block or a helmet: the client draws a flat item in the head slot as
a sprite on edge. An animal hat is a `PLAYER_HEAD` with the animal's `skull-texture`.

## Suits

A `pieces` block fills several slots at once:

```hocon
pieces {
  head  { material = "LEATHER_HELMET",     color = "#1e64c8" }
  chest { material = "LEATHER_CHESTPLATE", color = "#1e64c8" }
  legs  { material = "LEATHER_LEGGINGS",   color = "#1e64c8" }
  feet  { material = "LEATHER_BOOTS",      color = "#1e64c8" }
}
```

A slot may also name only its material: `head = "GOLDEN_HELMET"`. The slots are `head`, `chest`, `legs`, `feet` and
`off-hand`. `color` dyes leather only; `color = "rainbow"` re-dyes
the piece every frame. A resource pack suit uses `custom-model-data` on any material.

## Banners

A banner is worn on the head. Its `item` names a banner material and its `patterns`, drawn in order:

```hocon
item {
  material = "PURPLE_BANNER"
  patterns = [
    { pattern = "flower", color = "MAGENTA" }
    { pattern = "border", color = "BLACK" }
  ]
}
```

`pattern` is a banner pattern key such as `stripe_top` or `creeper`; `color` a dye colour. A banner without an `icon`
is shown in the menu as itself, with its patterns.

## Item keys

An item is a material name, or a block with `material`, `color`, `custom-model-data` and `skull-texture` (a base64
texture value).

Related: [Hotbar items](hotbar-items.md), [Category files](../config/category-files.md#worn-families)
