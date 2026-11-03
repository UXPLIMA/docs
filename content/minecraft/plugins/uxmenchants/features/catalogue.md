---
title: The shipped enchantments
order: 314
description: The twelve example enchantments, with rarity, type, targets, levels, trigger, numbers and routes.
icon: book-open
---

The plugin ships twelve enchantments as working examples. Every number is from the shipped files; change any of them in
`enchantments/<id>.conf`. Each node is `uxmenchants.enchantment.<id>`, given to everyone by default.

| Enchantment | Rarity | Type | Goes on | Max | Trigger | Effect |
|---|---|---|---|---|---|---|
| Blunted | Common | Enchantment | Sword | 3 | attack | `launch` the victim |
| Curse of Brittleness | Common | Curse | Armour | 2 | defend | `damage` the wearer |
| Cushioned | Common | Enchantment | Boots | 3 | defend | `heal` the wearer |
| Ember | Common | Enchantment | Sword, axe | 3 | attack | `ignite` the victim |
| Prospector | Common | Enchantment | Pickaxe, shovel | 3 | block-break | `extra-drops` |
| Scholar | Common | Enchantment | Pickaxe | 3 | block-break | `give-experience` |
| Sharpened | Common | Enchantment | Sword | 4 (7 overpowered) | attack | `damage` the victim |
| Smelter | Rare | Enchantment | Pickaxe | 1 | block-break | `smelt-drops` |
| Steadfast | Rare | Enchantment | Chestplate | 1 | defend | `extinguish` the wearer |
| Thunderstruck | Legendary | Enchantment | Sword | 2 | attack | `lightning` |
| Tree Feller | Rare | Enchantment | Axe | 3 | block-break | `tree-feller` |
| Vein Miner | Rare | Enchantment | Pickaxe | 3 | block-break | `vein-miner` |

## Blunted

"Trades an edge for a shove of `<power>` blocks." Launch `power` 0.4, plus 0.2 on a diminishing curve, capped at 1.2.
Conflicts with Sharpened and shares the `edge` group with it. Table, villager, loot and anvil; fishing, mob drops and
raids written off. Its `numbers` block works out `knockback-percent = power * 100`.

## Curse of Brittleness

"The armour gives, and so do you." On defence, a chance of 10, plus 10 per level, capped at 20, to deal the wearer 0.5,
plus 0.5, capped at 1. Not from the table or villagers; from loot and the anvil. A grindstone does not take it off, and an
item carries at most two curses.

## Cushioned

"Gives a little back when you are hit while wearing it." On defence, a chance of 20, plus 10, capped at 40, to heal the
wearer 1, plus 0.5 on a diminishing curve, capped at 2. Cooldown 5 seconds.

## Ember

"Sets what you hit alight, now and then." A chance of 20, plus 10, capped at 45, to set the victim alight for 2, plus 1,
seconds.

## Prospector

"Shakes a little more out of every block." A chance of 15, plus 10, capped at 40, to drop the block's drops again at a
multiplier of 2. Conflicts with Smelter.

## Scholar

"Turns broken stone into a little learning." A chance of 25 to give 1, plus 1, capped at 4, experience.

## Sharpened

"Cuts deeper with every level." Deals 1, plus 1 per level, extra damage. `overpower-max = 7`. Conflicts with Blunted;
group `edge`. The commented reference for the format.

## Smelter

"Cooks what you mine before it drops." Conflicts with Prospector.

## Steadfast

"Puts out a fire before it reaches you." On defence, puts the wearer out, with a 3 second cooldown.

## Thunderstruck

"Calls the sky down on what you hit." A chance of 5, plus 3, capped at 8, to strike lightning. Cooldown 8 seconds. Not
from villagers.

## Tree Feller

"Takes the whole trunk in one swing." Up to 12, plus 12, capped at 96, blocks of the eleven logs and stems (oak, spruce,
birch, jungle, acacia, dark oak, mangrove, cherry, pale oak, crimson, warped). Not from villagers.

## Vein Miner

"Follows an ore through the stone." Up to 6, plus 4, capped at 32, blocks of the same ore: coal, iron, copper, gold,
redstone, lapis, diamond and emerald with their deepslate forms, nether gold, nether quartz and ancient debris. Not from
villagers.

Related: [Enchantment files](enchantment-files.md), [Effects](effects.md)
