---
title: Item files
order: 502
description: Every key of an item file, with its default and what it does.
icon: package
---

`items/<id>.conf`. The file name is the id. Every key but `material` is optional.

| Key | Default | Meaning |
|---|---|---|
| `material` | required | Any material; an unknown name becomes `STONE` |
| `slot` | | 0 to 8 the hotbar, 9 to 35 the rest of the inventory, 36 to 39 the armour, 40 the off hand |
| `name` | the vanilla name | MiniMessage; a single space shows nothing |
| `lore` | none | MiniMessage lines; `@key` reads a line of the messages files |
| `custom-model-data` | `-1` | A resource pack model |
| `glow` | `false` | The shimmer without an enchantment |
| `unbreakable` | | Never wears out |
| `triggers` | `[]` | `join`, `first-join`, `respawn`, `world-change`; empty means only the give commands |
| `cooldown` | `items.default-cooldown` | Before a click works again |
| `restrictions.locked` | | Cannot be moved, dragged, swapped, dropped or placed; kept out of death drops |
| `skip-when` | `[]` | PlaceholderAPI texts that hold the item back |
| `regions` | `[]` | WorldGuard regions it is handed out in; empty is everywhere |
| `left-click` | `[]` | Action lines for a left click |
| `right-click` | `[]` | Action lines for a right click |

## Clicks

```hocon
left-click = [
  "[menu] menu open serverselector"
  "[sound] minecraft:item.book.page_turn 1 1.2"
]
right-click = [
  "[server] survival"
]
```

The verbs are in [Clicks](../features/clicks.md).

Related: [Items](../features/items.md)
