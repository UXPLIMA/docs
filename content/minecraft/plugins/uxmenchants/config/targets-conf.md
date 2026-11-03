---
title: targets.conf
order: 504
description: Extra enchantable items, named item sets, and every target with its slots and items.
icon: crosshair
---

## extra-enchantable-items

Items the enchanting table offers ours on beyond what vanilla enchants: `SHIELD`, `ELYTRA`, `SHEARS`,
`FLINT_AND_STEEL`.

## sets

A list written once and named with `@` wherever items are listed: `anvil-items = ["@swords", "SHIELD"]`.

| Set | Holds |
|---|---|
| `swords` | The six swords and the mace |
| `axes`, `pickaxes`, `shovels`, `hoes` | The six of each |
| `bows` | Bow, crossbow |
| `helmets` | The six helmets and the turtle shell |
| `chests` | The six chestplates and the elytra |
| `leggings`, `boots` | The six of each |
| `tools` | `@pickaxes`, `@shovels`, `@axes`, `@hoes` |
| `weapons` | `@swords`, `@axes`, `@bows` |
| `armour` | `@helmets`, `@chests`, `@leggings`, `@boots` |

A set may name another set, one level down. A name no set carries reaches the material reader, which refuses it with
the word in the message.

## targets

| Key | Meaning |
|---|---|
| `name` | What a player reads; a target with none reads as its id |
| `items` | The materials it covers |
| `slots` | `mainhand`, `offhand`, `hands`, `helmet`, `chestplate`, `leggings`, `boots`, `armor`, `any`; one or a list |

| Target | Reads | Slots |
|---|---|---|
| `sword` | Sword | `mainhand` |
| `axe` | Axe | `mainhand` |
| `pickaxe` | Pickaxe | `mainhand` |
| `shovel` | Shovel | `mainhand` |
| `bow` | Bow | `hands` |
| `helmet` | Helmet | `helmet` |
| `chestplate` | Chestplate | `chestplate` |
| `leggings` | Leggings | `leggings` |
| `boots` | Boots | `boots` |
| `armor` | Armour | `armor` |

Related: [Grindstone, types and targets](../features/grindstone-types-targets.md#targets-and-slots)
