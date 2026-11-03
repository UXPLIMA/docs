---
title: Menu files
order: 607
description: The keys of a window file, the tile line, and every action and source the windows know.
icon: layout-template
---

Every window in `menus/` is a file: move a slot, change a material, take a button out, and the plugin draws what you
wrote. Nothing about a window is decided in the code.

| Key | Meaning |
|---|---|
| `title` | `@menu.<window>.title`, or your own words |
| `rows` | One to six |
| `open-actions` | What runs when it opens: `sound:ITEM_BOOK_PAGE_TURN 0.7 1.2` |
| `layout`, `layout-char` | Rows of characters, and the character each item fills |
| `slot`, `slots` | Where an item sits; a range is `"10-16"`, a list mixes both |
| `priority` | Which item wins a slot two ask for; the highest is drawn last |
| `type` | `previous` or `next` turns the page of the list |
| `click` | `left`, `right`, `shift_left`, `shift_right` |
| `list` | `source` and `template` for the squares the plugin fills |
| `item-drag` | Turns a slot into one you drop an item into: `rules`, `consume`, `actions` |
| `decor { hide-tooltip = true }` | A pane with no tooltip |

`lore = ["tile:<colour> @<block> <facts>"]` draws a tile; see [Menus](../menus/overview.md#tiles).

## Actions and sources

| Name | Window | Does |
|---|---|---|
| `enchants:fitting` | Wiki | List source: what fits the drop slot |
| `enchants:inspecting` | Wiki | Condition: an item is in the drop slot |
| `enchants:inspect`, `enchants:inspect-held`, `enchants:clear` | Wiki | Read the dropped item, read the held one, empty the slot |
| `enchants:open-info` | Wiki | The clicked enchantment's page |
| `enchants:open-grouped` | Wiki | The grouped wiki |
| `enchants:groups` | Grouped | List source: the groups |
| `enchants:open-group` | Grouped | What is inside the clicked group |
| `enchants:open-wiki` | Grouped, page | Back to the wiki |
| `enchants:editor-list` | Editor | List source: every enchantment |
| `enchants:editor-properties` | Editor | The clicked enchantment's properties |
| `enchants:editor-new` | Editor | Asks for an id |

Every window also knows `close`, `open:<menu>`, `command:<line>`, `message:<line>` and
`sound:<name> <volume> <pitch>`.

The editor's property and list windows have files of their own shape; see [Editor windows](../menus/editor.md).

Related: [Menus](../menus/overview.md), [Messages](messages.md)
