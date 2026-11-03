---
title: Acquisition
order: 406
description: How the table, villagers, loot chests, fishing, mob drops and raids hand out enchantments, with caps and reductions.
icon: dice-5
---

Seven ways a player gets one of ours, each switched off on its own: the four that bound a roll here, the anvil, and books
an operator gives. Write `enabled = false` on all of them and enchantments come out of books only.

## A roll

Every enchantment that may come out of a route is a candidate, in the catalogue's order. Each is tested against its
rarity's chance on that route. Every grant after the first divides the chance of the ones that follow by the route's
`reduction`, and the roll stops at its `cap`.

| Route | `cap` | `reduction` | Ships |
|---|---|---|---|
| `table` | 2 | 3.0 | on |
| `villager` | 1 | 2.0 | on |
| `loot` | 2 | 2.5 | on |
| `fishing` | 1 | 2.5 | off |
| `mob-drops` | 1 | 2.5 | off |
| `raids` | 2 | 2.0 | off |

A reduction of 1.0 is no reduction. The cap is the knob that stops a table handing out five custom enchantments at once.

An enchantment is a candidate only when its file's `obtaining` allows that route, its rarity's chance on that route is
above zero, and the route's level reaches the rarity's `minimum-level`.

## The enchanting table

The vanilla table is kept exactly as it is. When a player enchants, a roll of ours runs at the experience cost they
paid and may add ours to the same item. An enchantment is a candidate when:

- the item is in its `table-items`, or in `extra-enchantable-items` of `targets.conf` for items vanilla does not
  enchant (a shield, an elytra, shears, flint and steel);
- the player holds its node and the node with `.table` after it (`uxmenchants.enchantment.sharpened.table`);
- the table level reaches its rarity's `minimum-level`: common 1, rare 10, legendary 25.

<Callout type="warning" title="The .table nodes are not declared">

The plugin does not declare the `.table` nodes, so by default only an operator holds them and a player is never offered
one of ours at the table. Give them to your players with your permission plugin, for example
`uxmenchants.enchantment.*` in LuckPerms, which covers every enchantment's node and its `.table` node.

</Callout>

The level it comes out at is the table level scaled onto its range: `max-level * table level / 30`, rounded up.

## The other routes roll at level one

A villager, a chest, a catch, a mob and a raider roll at level 1. So only a rarity whose `minimum-level` is 1 (the
shipped `common`) comes out of them, always at level 1, and the shipped `villager-chance` and `loot-chance` of `rare` and
`legendary` are never reached.

## Villagers

A villager who acquires a trade selling an enchanted book may sell one of ours instead. The price, the uses and the
villager's experience are untouched; only the result changes. Only the first grant is used.

## Loot chests

Every item a loot table puts in a chest is offered to the roll, so a sword in a dungeon chest may come out carrying ours.
Nothing is added to the chest and nothing is taken out.

## Fishing, mob drops and raids

Each works like the chest on what is caught or dropped. A raider (a pillager, a vindicator, an evoker) is its own route
rather than a mob drop, so a raid can be worth fighting without making every zombie drop the same thing. A rarity that
writes no `fishing-chance`, `mob-drop-chance` or `raid-chance` uses its `loot-chance`.

## Worlds

In a world that is off, no route hands anything out. See [Protection](protection.md#worlds).

Related: [rarities.conf](../config/rarities-conf.md), [config.conf](../config/config-conf.md#acquisition)
