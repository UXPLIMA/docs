---
title: Categories
order: 408
description: The tabs of the house, item id patterns, order, the fallback, and the shipped ten.
icon: folder
---

Each block of `categories.conf` is a tab.

| Key | Meaning |
|---|---|
| `order` | Where the tab sits, lowest first, and who takes an item two tabs would accept: the lowest wins. Equal numbers are decided by the name |
| `icon` | The item the tab is drawn with |
| `matches` | Item ids; `*` stands for any run of characters. A name without a namespace is read as `minecraft:` |

An item no tab accepts goes to the `fallback` category, which keeps a modded item in the house rather than out of it.

A listing is put into a category once, when it is written, so a tab opens with one indexed query however large the house
is. A change to the file applies to new listings.

## Shipped

| Tab | Order | Icon | Takes, for example |
|---|---|---|---|
| `weapons` | 10 | diamond sword | `*_sword`, bows, crossbows, tridents, maces, shields, arrows |
| `tools` | 20 | diamond pickaxe | `*_pickaxe`, `*_axe`, `*_shovel`, `*_hoe`, shears, rods, compass, clock, horse armour, lead, name tag |
| `armour` | 30 | diamond chestplate | `*_helmet`, `*_chestplate`, `*_leggings`, `*_boots`, elytra, trim templates |
| `ores` | 40 | diamond | `*_ore`, `raw_*`, `*_ingot`, `*_nugget`, gems and their blocks |
| `food` | 50 | cooked beef | Food |
| `brewing` | 60 | brewing stand | Potions and ingredients |
| `redstone` | 70 | redstone | Redstone parts |
| `blocks` | 80 | grass block | Building blocks |
| `spawn-eggs` | 90 | pig spawn egg | `*_spawn_egg` |
| `misc` | 100 | chest | Everything else: the `fallback` |

Delete a category and it is gone; a server that sells only fish keeps one.

Related: [categories.conf](../config/categories-conf.md), [Category window](../menus/category.md)
