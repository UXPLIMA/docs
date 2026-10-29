---
title: Basic Concepts
order: 14
description: Ownership, how a spawner runs, its two modes, levels and what one cycle produces.
icon: lightbulb
---

## A spawner belongs to a player

A uxmSpawners spawner is a spawner item that carries its type. Placing it registers it to the player
who placed it. Only that owner, and the friends they add, can open its menu; anyone else who
right-clicks it gets nothing. Only the owner, or staff with
`uxmspawners.breakallspawners`, can break it.

## It runs on its own timer

The vanilla spawner logic is switched off for these blocks. Instead, the plugin runs every spawner
every `delay` seconds, as long as:

| Condition | Note |
|---|---|
| The chunk is loaded | An unloaded chunk produces nothing |
| A player is within `range` blocks | Any player, not only the owner. A WildLoaders chunk loader in the chunk counts too |

The vanilla rules do not apply: light, free space and the number of mobs nearby make no difference.

## Two modes

| `mode` | Produces |
|---|---|
| `ENTITY` | Mobs, or their loot directly with auto-kill |
| `ITEM` | An item, such as the shipped `diamond` spawner. Auto-kill and XP do not apply |

The type of a spawner is set by its file entry. See [Spawner Types](../config/spawner-types.md).

## Levels

Identical spawners stack: the owner opens the menu, presses **Level Up**, and every matching spawner
in their inventory is merged into the placed one, one level each. A spawner at level 5 produces what
five separate spawners would, from one block and one storage. See [Levels](../features/levels.md).

## What one cycle produces

| Setup | Result |
|---|---|
| `ENTITY` with auto-kill, modern | No mob appears. The loot of one mob, times the level, times (1 + spawn-amount boost), goes straight to storage |
| `ENTITY` with auto-kill, classic | 1 + spawn-amount boost mobs appear and die at once. Each one's drops are multiplied by the level |
| `ENTITY` without auto-kill | 1 + spawn-amount boost mobs appear. When anyone kills one, its drops are multiplied by the level |
| `ITEM` | Level times (1 + spawn-amount boost) items |

Boosts are on [Boosts](../features/boosts.md), auto-kill on [Auto-Kill](../features/auto-kill.md).

## Storage

With virtual storage on, which is the default, everything goes into the spawner: items up to the
storage limit for each item type, and XP up to the XP limit. What does not fit is lost. The owner
withdraws or sells it from the menu. With it off, items and XP drop on the ground around the
spawner. See [Storage & Selling](../features/storage.md).

Mobs from a spawner drop what your [loot table](../config/loot-tables.md) for that mob says, or their
normal drops if there is none, however they die.

## What is kept

Placed spawners, their levels, friends, boosts and storage are kept in `database.db`. Changes are
written every `auto-save.interval` seconds and when the server stops.

Breaking a spawner at level 1 removes it with everything in its storage. Empty the storage first.
