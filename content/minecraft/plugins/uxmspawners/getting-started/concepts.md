---
title: Basic Concepts
order: 114
description: Ownership, how a spawner runs, its two modes, levels and what one cycle produces.
icon: lightbulb
---

## A spawner belongs to a player

A uxmSpawners spawner is a spawner item that carries its type. Placing it registers it to the player
who placed it. Only that owner, and the friends they add, can open its menu; anyone else who
right-clicks it gets nothing. Only the owner, or staff with `uxmspawners.breakallspawners`, can
break it. With a claim plugin installed, the claim has to let them too.

Spawner items, and the spawner pickaxe, are signed by the server. An item made or edited in creative
mode or with an NBT editor does not work. See [Item Signing](../features/item-signing.md).

## It runs on its own timer

The block's own vanilla spawning is switched off. Instead, the plugin runs every spawner every
`delay` seconds, as long as:

| Condition | Note |
|---|---|
| The chunk is loaded | An unloaded chunk produces nothing |
| A player is within `range` blocks | Any player, not only the owner. A WildLoaders chunk loader in the chunk counts too |
| The spawner is not stopped | The owner can stop it from the menu |

The vanilla rules do not apply: light, free space and the number of mobs nearby make no difference.
See [Production](../features/production.md).

## Two modes

| `mode` | Produces |
|---|---|
| `ENTITY` | Mobs, or their loot directly with Auto Kill |
| `ITEM` | An item, such as the shipped `diamond` spawner. Auto Kill and XP do not apply |

The type of a spawner is set by its file in `spawners/`. See [Spawner Types](../config/spawner-types.md).

## Levels

Identical spawners stack: the owner opens the menu, presses **Level Up**, and every matching spawner
in their inventory is merged into the placed one, one level each. A spawner at level 5 produces what
five separate spawners would, from one block and one storage. See [Levels](../features/levels.md).

## What one cycle produces

| Setup | Result |
|---|---|
| `ENTITY` with Auto Kill, modern | No mob appears. The loot of one mob, times the level, times (1 + Spawn Amount boost), goes straight to storage |
| `ENTITY` with Auto Kill, legacy | 1 + Spawn Amount boost mobs appear and die at once. Their drops are multiplied by the level |
| `ENTITY` without Auto Kill | 1 + Spawn Amount boost mobs appear. When anyone kills one, its drops are multiplied by the level |
| `ITEM` | Level times (1 + Spawn Amount boost) items |

`level-scaling.mode: MOB_COUNT` spawns level times as many mobs instead of multiplying their drops.
Boosts are on [Boosts](../features/boosts.md), Auto Kill on [Auto Kill](../features/auto-kill.md).

## Storage

With Virtual Storage on, which is the default, everything goes into the spawner: items up to the
storage limit for each item type, and XP up to the XP limit. What does not fit is lost. The owner
takes or sells it from the menu. With it off, items and XP drop on the ground around the spawner.
See [Storage & Selling](../features/storage.md).

Mobs from a spawner drop into its storage however they die, multiplied by its level.

## What is kept

Placed spawners, their levels, friends, boosts and storage are kept in the database, `database.db`
by default. What players do is written at once; what spawners produce is written every
`flush-interval-seconds`, 5 by default, and when the server stops.

Breaking a spawner's last level is refused while its storage holds anything, unless
`break-with-storage` says otherwise. See [Breaking Rules](../features/breaking.md#storage-when-a-spawner-goes).
