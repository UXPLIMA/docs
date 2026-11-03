---
title: Key files
order: 611
description: Every key of keys/<id>.conf, with its default and meaning.
icon: key-round
---

One file per key under `keys/`; the file name is the id. `common-key.conf` and `elite-key.conf` ship.

| Key | Default | Meaning |
|---|---|---|
| `name` | | The key's name, in the three shapes |
| `item.material` | | The physical key's item |
| `item.amount` | `1` | How many one key item is |
| `item.glow` | `false` | The glint |
| `item.lore` | `[]` | Lines under the name |
| `item.item-model`, `item.model-data` | none | A resource pack's model |
| `tradeable` | `false` | Players may send it with `/crates send` |
| `crafting.enabled` | `false` | Register a bench recipe |
| `crafting.shapeless` | `false` | Ignore positions |
| `crafting.amount` | `1` | Keys one craft makes |
| `crafting.permission` | `""` | Needed to see the result |
| `crafting.recipe` | | Nine entries, left to right and top to bottom, `air` for empty |

## menu

| Key | Default | Meaning |
|---|---|---|
| `order` | | The tile's place in `/crates keys`, lowest first |
| `hidden` | `false` | Keep it off the window |
| `click.left`, `click.right` | `[]` | Effect lines run on a click |
| `store` | `[]` | Lines shown to a player who holds none |

Related: [Keys](../features/keys.md), [Keys window](../menus/keys.md)
