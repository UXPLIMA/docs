---
title: "Guide"
order: 10
description: "How uxmEnchants works, how to set it up and what each file holds."
---

uxmEnchants adds custom enchantments, each written as one file, and weaves them into the enchanting table, villager trades, loot chests, the anvil and books. Rarities decide how they are found, types decide how they are drawn and whether a grindstone takes them off, and targets decide when they fire.

## Install

1. Put the jar in `plugins/`.
2. Start the server once. The plugin writes its files into `plugins/uxmEnchants/`, including eight example enchantments.
3. Edit or add files under `enchantments/`, then run `/uxmenchants reload`.

PlaceholderAPI, an economy and the claim plugins are optional.

When any content file is wrong, the reload is refused as a whole: the console names the file, the key and the line, and the server keeps the content it had.

## Files

| File | What it holds |
|---|---|
| `config.conf` | storage, item data, limits, acquisition routes, drag and drop, anvil, grindstone, worlds, books, display, charges, cooldowns, effects, claims, editor |
| `enchantments/<id>.conf` | one enchantment per file; the file name is the id |
| `rarities.conf` | how likely each rarity is on each route, and the table level it starts at |
| `types.conf` | how each type is drawn, how many one item may carry, whether a grindstone removes it |
| `targets.conf` | the item lists and equipment slots an enchantment fires from |
| `vanilla.conf` | how vanilla enchantments are drawn beside ours |
| `editor/new-enchantment.conf` | the template the in-game editor starts from |
| `menus/*.conf`, `commands.conf`, `messages/`, `theme.conf` | windows, commands, words and colours |

## An enchantment

`enchantments/sharpened.conf` is commented in full and is the reference for the format. In short:

| Key | Meaning |
|---|---|
| `name`, `description` | a line of the messages files (`"@content.enchantments.sharpened.name"`), drawn in each reader's language; one line per language (`en = "..."`, `tr = "..."`); or one line everybody reads. The shipped files use the first, with their words under `content` in each messages file |
| `rarity`, `type`, `targets` | ids from `rarities.conf`, `types.conf` and `targets.conf` |
| `max-level` | the highest level; `0` or less means no ceiling |
| `overpower-max` | how far the anvil may push it past `max-level` when `anvil.overpowering` is on |
| `permission` | the node a player needs to fire it; `uxmenchants.enchantment.<id>` by default |
| `anvil-items`, `table-items` | items the anvil puts it on, and items the table offers it on |
| `conflicts` | enchantments it refuses to sit beside; a name with a colon is another plugin's |
| `required` | enchantments that must already be on the item |
| `removed` | enchantments that come off when this goes on |
| `exclusive-groups` | only one enchantment of a group may be on an item |
| `obtaining` | `table`, `villager`, `loot`, `anvil`, each on or off |
| `trigger` | `attack`, `defend`, `kill`, `death`, `block-break`, `block-place`, `shoot-bow`, `projectile-hit`, `interact`, `fish`, `move` or `periodic` |
| `effects` | a list of effect blocks |

An effect block names an `effect` and its arguments, and may carry its own `chance`, `cooldown`, `delay`, `conditions` and `actions`. The effects are `damage`, `heal`, `feed`, `ignite`, `potion`, `launch`, `pull`, `extinguish`, `lightning`, `explode`, `vein-miner`, `tree-feller`, `smelt-drops`, `extra-drops`, `repair-item` and `give-experience`.

Every number is either fixed or a modifier block that scales with the level:

```hocon
amount { base = 1.0, per-level = 0.5, cap = 4.0, mode = add, curve = linear }
```

`mode` is `add` (`base + per-level * growth`) or `multiply` (`base * (1 + per-level * growth)`). `curve` is `linear` (growth `level - 1`), `diminishing` (the square root of that) or `exponential` (`2^(level-1) - 1`). `cap` is the value it never goes above. There is no free maths expression.

A condition is a comparison (`"%player_level% >= 10"`), a balance (`"money >= 250"`) or an item count (`"item DIAMOND >= 2"`). An action is `[message]`, `[actionbar]`, `[title]`, `[sound]`, `[broadcast]`, `[close]`, `[console]`, `[player]`, `[take-money]` or `[take-item]`. A condition on money together with `[take-money]` makes an enchantment cost money to fire.

## What may go on an item

When an enchantment is added by any route, it is refused when:

- the level is above `max-level` (or `overpower-max` on an overpowering anvil);
- it conflicts with something on the item, from either side, or shares an exclusive group with it;
- a `required` enchantment is missing;
- the item is at its limit and this would be a new enchantment (raising a level is always allowed);
- its type is at that type's `limit-per-item`.

`removed` enchantments come off first, so an evolution can replace the enchantment it grows from even on a full item.

The limit is `limit.base` for everyone, raised by a node `limit.node-prefix` plus a number (`uxmenchants.limit.6`), never above `limit.maximum-from-node`. It counts ours only, never vanilla ones. `/enchants limit` prints the whole calculation for the item in hand.

## Getting enchantments

`rarities.conf` sets, per rarity, `minimum-level` (the table level it starts at) and the chance out of a hundred on each route: `table-chance`, `villager-chance`, `loot-chance`, and optionally `fishing-chance`, `mob-drop-chance` and `raid-chance` (they default to `loot-chance`). A chance may be a modifier block. A chance of `0` closes that route for that rarity.

`acquisition` in `config.conf` bounds each route: `enabled`, `cap` (the most enchantments one roll gives) and `reduction`, which divides the chance of every grant after the first. `fishing`, `mob-drops` and `raids` ship off.

The anvil applies books at `anvil.base-cost` levels for the first of ours, multiplied by `cost-exponent` for each further one, unless `vanilla-costs` is on. `drag-and-drop` (off by default) lets a player click a book onto an item for `cost` levels, times the book's level when `per-level` is on. `grindstone.enabled` decides whether ours come off at all; the type decides which.

## Types and targets

A type in `types.conf` has a `name`, `order`, a `format` with `<name>`, `<roman>` and `<number>`, a `limit-per-item` (`-1` for no limit) and `grindstone-removable`. A curse is a type that a grindstone does not remove.

A target in `targets.conf` has `items` and `slots` (`mainhand`, `offhand`, `hands`, `helmet`, `chestplate`, `leggings`, `boots`, `armor`, `any`). An enchantment fires only while its item is used from one of its targets' slots. `extra-enchantable-items` lets the table offer ours on items vanilla does not enchant.

## Other settings

| Block | Meaning |
|---|---|
| `worlds` | `mode` (`blacklist` or `whitelist`) and `list`; in a world that is off nothing fires and nothing is handed out |
| `books` | the book's `name-format`, `lore` and `glow` |
| `display` | how enchantments are drawn on items; the real item is never changed. Players toggle descriptions with `/enchants descriptions` |
| `charges` | off by default; items spend `consume` charges per firing up to `maximum`, and one `fuel-material` item in the anvil puts `recharge` back; `bands` set the tooltip line |
| `cooldowns.persist` | keep cooldowns across restarts |
| `block-break-treaty` | skip blocks another vein miner already broke, and mark ours |
| `effects.materials` | the blocks block effects may touch; `BEDROCK`, `SPAWNER` and a few more are never touched |
| `claims` | respect land claims of the nineteen supported plugins; `combine` is `any-land` or `all-land` |
| `editor.enabled` | the in-game editor |

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/enchants` | `uxmenchants.use` | open the wiki |
| `/enchants groups` | `uxmenchants.use` | the grouped wiki |
| `/enchants info <enchantment>` | `uxmenchants.use` | one enchantment's page |
| `/enchants list` | `uxmenchants.use` | one line per enchantment this server defines |
| `/enchants limit` | `uxmenchants.use` | the limit calculation for the item in hand |
| `/enchants descriptions` | `uxmenchants.use` | turn descriptions on items on or off for you |
| `/enchants lang ...` | `uxmenchants.language`, `uxmenchants.admin.language` | languages |
| `/uxmenchants apply <enchantment> [level]` | `uxmenchants.admin.apply` | put an enchantment on the item in hand |
| `/uxmenchants remove <enchantment>` | `uxmenchants.admin.remove` | take one off the item in hand |
| `/uxmenchants give <player> <enchantment> [level]` | `uxmenchants.admin.give` | give a book |
| `/uxmenchants giverandom <player> <group> [minimum-level] [maximum-level]` | `uxmenchants.admin.give` | give a random book from a rarity or type |
| `/uxmenchants export` | `uxmenchants.admin.export` | what the held item carries |
| `/uxmenchants strip <world>` | `uxmenchants.admin.strip` | remove ours from every player in a world |
| `/uxmenchants import <lore\|registry>` | `uxmenchants.admin.import` | convert another plugin's enchantments on online players' items into ours |
| `/uxmenchants editor` | `uxmenchants.admin.editor` | the in-game editor |
| `/uxmenchants reload`, `version`, `doctor` | `uxmenchants.admin.reload`, `.version`, `.doctor` | the usual operator commands |

`import lore` reads lore lines written by plugins that store enchantments as lore; `import registry` reads `minecraft:` enchantment keys that name one of ours. An item that matches nothing is left as it was.

The names and aliases come from `commands.conf`. The [reference](reference.md) lists the default aliases and every permission.

## Placeholders

| Placeholder | Value |
|---|---|
| `%uxmenchants_held_count%` | how many of ours the held item carries |
| `%uxmenchants_held_rarity_<id>%`, `%uxmenchants_held_type_<id>%` | how many of that rarity or type it carries |
| `%uxmenchants_held_level_<id>%` | the level of that enchantment on it |
| `%uxmenchants_limit_total%`, `_limit_used%`, `_limit_left%` | the player's limit, what the held item uses, what is left |
| `%uxmenchants_charges%`, `%uxmenchants_charges_max%` | charges left on the held item, and the most it holds |
| `%uxmenchants_cooldown_<id>%` | seconds left on that cooldown |
| `%uxmenchants_defined%`, `%uxmenchants_defined_rarity_<id>%` | how many enchantments the server defines, in all or of one rarity |

## Troubleshooting

- **A reload is refused.** The console names the file, key and line; the old content stays loaded.
- **An enchantment is struck through on an item.** The player lacks its permission.
- **The table never offers an enchantment.** Its rarity's `table-chance` is `0`, the table level is below `minimum-level`, the item is not in `table-items`, or `obtaining.table` is off.
- **Too many enchantments come out of one table roll.** Lower `acquisition.table.cap` or raise `reduction`.
