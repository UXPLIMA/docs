---
title: Wiki
order: 502
description: The window /enchants opens, with a slot you drop an item into to see what fits it.
icon: book
---

`/enchants` opens `menus/wiki.conf`: six rows, titled "Enchantments", with a page turn sound.

| Slot | Item | Click |
|---|---|---|
| 4 | The drop slot: the item dropped in it, or a pane | Drop an item to filter; left click reads the item in your hand; right click empties it |
| 9 to 44 | One enchanted book per enchantment that fits | Left: open its [page](info.md) |
| 48 | Arrow, previous page | |
| 49 | Bookshelf, "Group them" | Opens the [grouped wiki](grouped.md) |
| 50 | Arrow, next page | |
| 53 | Barrier, close | |

The rest is gray panes.

## The drop slot

Empty, its tile reads "Drop an item here" with how many enchantments the server has. Drop a pickaxe, a sword or a piece
of armour in it and the list below shows only what fits that item; the tile says "Everything below fits this item" and
how many do. The item is not taken.

A Floodgate form has no slot to drop into, so a left click reads the item in the player's hand instead, which works on
both clients, and an empty hand clears the filter. Take the left click off the drop item and a Bedrock player loses the
filter.

## An entry

Each tile shows the name, the rarity, the description, the type, the highest level, what it goes on and what it
refuses, and "Click to read the whole page".

| Token | Value |
|---|---|
| `%wiki_item%` | The item in the drop slot as a material, for the slot's `material` |
| `%wiki_item_name%` | The same item in words |
| `%wiki_count%` | How many fit it, or how many there are with the slot empty |
| `%wiki_state%` | `empty` or `filled`: which block of the messages the slot reads |

| Name | Kind | Does |
|---|---|---|
| `enchants:fitting` | List source | Every enchantment that fits the drop slot's item |
| `enchants:inspecting` | Condition | An item is in the drop slot |
| `enchants:inspect` | Action | Reads the item dropped |
| `enchants:inspect-held` | Action | Reads the item in hand; an empty hand clears |
| `enchants:clear` | Action | Empties the slot |
| `enchants:open-info` | Action | Opens the clicked enchantment's page |
| `enchants:open-grouped` | Action | Opens the grouped wiki |

Related: [Grouped wiki](grouped.md), [Enchantment page](info.md)
