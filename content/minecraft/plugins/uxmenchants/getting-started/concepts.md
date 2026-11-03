---
title: Concepts
order: 103
description: The item is the truth, enchantment files, rarities, types, targets, triggers, routes and the limit.
icon: lightbulb
---

## The item is the truth

Our enchantments are not in the server's registry. An item carries them as data under one key
(`uxmenchants:enchantments`), each as an id and a level, and nothing else of ours is written onto it. The lines a player
reads are drawn on a copy of the item every time it is shown, so renaming an enchantment changes every item on the
server at once, and nothing of ours can corrupt a lore another plugin is about to save.

Because the enchantment is not in the registry, the table, the anvil, the grindstone, villager trades, loot and books
are the plugin's own work on top of vanilla, and the database never holds an enchantment.

## The five kinds of content

| Kind | Where | Decides |
|---|---|---|
| Enchantment | `enchantments/<id>.conf` | What it is called, what it goes on, how high it goes, when it fires and what it does |
| Rarity | `rarities.conf` | How likely it is on each route, and the table level it starts at |
| Type | `types.conf` | How it is drawn, how many one item may carry, whether a grindstone takes it off |
| Target | `targets.conf` | The items it goes on, and the slot those items fire from |
| Vanilla | `vanilla.conf` | How vanilla enchantments are drawn beside ours |

A rarity is a weight and nothing else; a type is presentation and rule. Keeping them apart lets you reprice a rarity
without repainting every enchantment in it. A curse is a type whose grindstone rule is off, not a concept in the code.

## Triggers and slots

An enchantment names one trigger: `attack`, `defend`, `kill`, `death`, `block-break`, `block-place`, `shoot-bow`,
`projectile-hit`, `interact`, `fish` or `move`. It fires only while its item is used from one of its targets' slots, so
a boots enchantment fires when the boots are worn and never when they are held.

## Routes

An enchantment reaches an item through a route: `table`, `villager`, `loot`, `anvil` (a book or another item), and
three that ship off, `fishing`, `mob-drops` and `raids`. Every route, the in-game book and `/uxmenchants apply` pass
through one rule, so the limit and the conflicts are true everywhere at once.

## The limit

One item carries at most `limit.base` (3) of ours, raised per player by `uxmenchants.limit.<number>`. Vanilla
enchantments never count. Raising the level of one already on the item is always allowed. `/enchants limit` prints the
whole calculation.

## Content reloads whole

A reload validates every content file before it replaces anything. One wrong file refuses the whole reload and names
the file, the key and the line; the server keeps the content it had and never loads part of a set.

Related: [Enchantment files](../features/enchantment-files.md), [Applying](../features/applying.md)
