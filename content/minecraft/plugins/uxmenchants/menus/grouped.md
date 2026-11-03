---
title: Grouped wiki
order: 403
description: One tile per rarity, type and target instead of one per enchantment.
icon: library
---

`/enchants groups`, or the bookshelf of the wiki, opens `menus/grouped.conf`: six rows, titled "Enchantments by group".
A server with a hundred enchantments is unreadable as a flat list.

| Slot | Item | Click |
|---|---|---|
| 10 to 16, 19 to 25, 28 to 34 | A book per group | Left: what is inside the group |
| 48 | Arrow, previous page | |
| 49 | Enchanted book, back | The wiki |
| 50 | Arrow, next page | |
| 53 | Barrier, close | |

The groups are built from the content: every rarity of `rarities.conf`, every type of `types.conf` and every target of
`targets.conf`, in that order. Add a rarity and a tile appears on the next reload. A group with nothing in it is not
drawn.

| Token | Value |
|---|---|
| `%group_name%` | What the group is called, in the reader's language |
| `%group_kind%` | By rarity, by type, or by what it goes on |
| `%group_count%` | How many enchantments are in it |

| Name | Kind | Does |
|---|---|---|
| `enchants:groups` | List source | Every group with something in it |
| `enchants:open-group` | Action | Opens what is inside the clicked group |
| `enchants:open-wiki` | Action | Back to the wiki |

Related: [Wiki](wiki.md)
