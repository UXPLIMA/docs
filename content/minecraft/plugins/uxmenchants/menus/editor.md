---
title: Editor windows
order: 505
description: The editor list, the property editor of one enchantment, and the list window.
icon: pencil-ruler
---

All three need `uxmenchants.admin.editor` and are off when `editor.enabled` is false.

## The list

`menus/editor.conf`, six rows titled "Enchantments", drawn from a layout:

```hocon
layout = [
  "#########"
  "#xxxxxxx#"
  "#xxxxxxx#"
  "#xxxxxxx#"
  "#xxxxxxx#"
  "<###b##+>"
]
```

| Slot | Item | Click |
|---|---|---|
| 10 to 16, 19 to 25, 28 to 34, 37 to 43 | An enchanted book per enchantment, with its whole tooltip | Left: its properties |
| 45 | Arrow, previous | |
| 49 | Feather, back | Closes |
| 52 | Lime dye, new | Asks for an id |
| 53 | Arrow, next | |

`placeholders.tile-colour` (5) is the colour of the tiles. Tokens: `%editor_enchant%` (the name in the viewer's
language), `%tile_editor_enchant%` (the whole tooltip).

## Properties

`menus/editor-enchant.conf` is not shaped like the other window files, but the rows, slots and materials are there:

| Key | Ships as |
|---|---|
| `rows` | 6 |
| `property-slots` | 10 to 16, 19 to 25, 28 to 34, 37 to 43 |
| `back-slot`, `back-icon` | 49, `FEATHER` |
| `delete-slot`, `delete-icon` | 53, `BARRIER` |

Every property needs a slot; add a row before adding a setting. A file that is missing or wrong costs the layout, not
the editor: the shipped numbers are used and the reason goes to the console. The buttons are on
[In-game editor](../features/editor.md#the-properties).

## Lists

`menus/editor-list.conf` edits a list of lines (targets, anvil items, table items, conflicts) and is where a rarity or a
type is picked.

| Key | Ships as |
|---|---|
| `rows` | 6 |
| `entry-slots` | 10 to 16, 19 to 25, 28 to 34 |
| `add-slot`, `add-icon` | 53, `LIME_DYE` |
| `back-slot`, `back-icon` | 45, `FEATHER` |
| `entry-icon` | `PAPER` |

Left click a line to change it, right click twice to remove it, and the lime dye adds one.

Related: [In-game editor](../features/editor.md)
