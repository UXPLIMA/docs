---
title: OneBlock
order: 302
description: One block that comes back as something else, through phases.
icon: square
---

A OneBlock island starts as one block. Break it and it comes back as something else, drawn from the phase the island
has reached. Each phase lasts a number of breaks, counted over the whole life of the island, and brings blocks,
creatures and chests of its own.

## Phases

| Phase | What it holds |
|---|---|
| Plains | Grass, dirt and wood to build with, farm animals, and a treasure chest at break 100 |
| Underground | Stone and the first ores: coal, iron, copper and gold, with barrels of treasure |
| Snowfields | Snow, ice and spruce, with lapis in the cold |
| Ocean Floor | Sand, clay and prismarine, sea lanterns and sponges, and the odd wreck chest |
| Jungle | Jungle wood, moss, melons and bamboo, with emeralds hidden in it |
| The Nether | Netherrack, quartz and gold, glowstone, and now and then ancient debris |
| The End | End stone, obsidian and purpur, with diamonds at the very end |

Each phase is a file, `oneblock/phases/<name>.conf`: its icon, its length, the blocks it draws with their weights,
the creatures that may appear, its chests and its moments (a line over the block at a given break). The order of
the phases is the list in [oneblock.conf](../config/oneblock-conf.md). A phase may name blocks and creatures of
ItemsAdder, Nexo, CraftEngine and MythicMobs with the plugin's word in front.

A phase may ask for something before it begins, such as money in the bank, items or an island level; the island
waits at the end of the phase before until it is met. A phase may branch into ways the team picks from. A phase may
wait a while once it begins, with a countdown over the block.

## Playing

| Command | What it does |
|---|---|
| `/is oneblock` | Opens the [OneBlock window](../menus/game-modes.md#oneblock) |

Progress shows on a boss bar or the action bar, chosen in the OneBlock window. `/is reload` reads the phase files
again and every island goes on where it stands.

## Staff

`/isadmin oneblock setphase`, `setcount`, `addcount`, `check` and `respawn`; see [Staff](../commands/admin.md#isadmin).
Placeholders: `%uxmskyblock_oneblock_<value>%`, on [Placeholders](../placeholders/reference.md).
