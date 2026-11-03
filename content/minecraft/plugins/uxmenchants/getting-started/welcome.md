---
title: Welcome
order: 201
description: What uxmEnchants does, who it is for, and where to read next.
icon: hand
---

uxmEnchants adds custom enchantments to a Paper or Folia server. Each enchantment is one file. The plugin weaves them
into the enchanting table, villager trades, loot chests, the anvil and books, and it keeps the vanilla table exactly as
it is: a player enchants as they always did, and a roll of ours may add one of our enchantments to the same item.

Rarities decide how an enchantment is found. Types decide how it is drawn and whether a grindstone takes it off.
Targets decide which items it goes on and from which equipment slot it fires.

## What it does

| Part | What it gives you |
|---|---|
| [Enchantment files](../features/enchantment-files.md) | One file per enchantment: name, rarity, type, targets, levels, routes, trigger and effects |
| [Effects](../features/effects.md) | Sixteen effects, from `damage` to `vein-miner`, each with its own chance, cooldown and delay |
| [Numbers](../features/numbers-and-descriptions.md) | Every number fixed or scaled by level, and descriptions that name the number a level buys |
| [Conditions and actions](../features/conditions-and-actions.md) | A balance, an item count or a placeholder before an effect fires, and messages, sounds or a price after |
| [Applying](../features/applying.md) | One rule for every route: limit, conflicts, requirements, exclusive groups, type limits |
| [Acquisition](../features/acquisition.md) | The table, villagers, loot chests, and optionally fishing, mob drops and raids |
| [Anvil and books](../features/anvil-and-books.md) | Books, combining, a flat or steep cost, overpowering, and applying a book by clicking |
| [Display](../features/display.md) | Names, descriptions and vanilla enchantments drawn on a copy of the item, in each reader's language |
| [Charges and cooldowns](../features/charges-and-cooldowns.md) | Items that run down and are refilled with fuel, and cooldowns that survive a restart |
| [Protection](../features/protection.md) | World lists, nineteen claim plugins, the block break treaty and blocks no effect touches |
| [In-game editor](../features/editor.md) | Make and change enchantments from inside the game |
| [Migrating](../features/migrating.md) | Bring another plugin's enchantments across, strip ours from a world, export what an item carries |

Every line a player reads is in a language file, every window is a file, and no number is decided in the code.

## Read next

- [Installation](installation.md): the jar and the files.
- [Concepts](concepts.md): the item is the truth, rarities, types, targets and routes.
- [Your first enchantment](first-enchantment.md): a sword enchantment that heals on a kill.
- [The shipped enchantments](../features/catalogue.md): the twelve that ship, number by number.

Related: [Commands](../commands/overview.md), [Enchantment files](../features/enchantment-files.md)
