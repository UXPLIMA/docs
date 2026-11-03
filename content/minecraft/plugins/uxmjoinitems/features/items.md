---
title: Items
order: 301
description: How an item looks, the house tooltip, language keys and the three shipped items.
icon: package
---

An item is a file under `items/`. Only `material` is required. Every key is in [Item files](../config/item-files.md).

## Looks

| Key | Meaning |
|---|---|
| `material` | Any material this server knows. An unknown name becomes `STONE` and says so once in the console |
| `name` | MiniMessage. Left out, the vanilla name. A single space shows nothing: the line of air above a tooltip's title |
| `lore` | MiniMessage lines. A line that starts with `@` is a line of the messages files, in the player's language |
| `custom-model-data` | A resource pack model; `-1` or left out for the ordinary look |
| `glow` | The enchanted shimmer, without an enchantment |
| `unbreakable` | Never wears out |

An empty `name = ""` is not the same as a single space: the client then falls back to the name of the material.

## The house tooltip

The shipped items write their lore from the messages files, block by block: a title, the crumb under it, a description
header, two lines, and the line that says what a click does.

```hocon
name = " "
lore = [
  "@item.selector.title"
  "@item.navigation"
  " "
  "@item.description"
  "@item.selector.line-1"
  "@item.selector.line-2"
  " "
  "@item.selector.action"
  " "
]
```

## The shipped items

| Item | Material | Slot | Triggers | Cooldown | Click |
|---|---|---|---|---|---|
| `selector` | `COMPASS` | 0 | join, respawn, world-change | 1s | Left or right: `menu open serverselector` and a page turn |
| `cosmetics` | `ENDER_CHEST`, glowing | 4 | join, respawn | the default | Right: `cosmetics` |
| `lobby` | `CLOCK` | 8 | join, respawn, world-change | 1s | Left or right: `lobby` and a page turn |

All three are locked and unbreakable.

Related: [Item files](../config/item-files.md), [Clicks](clicks.md)
