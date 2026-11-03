---
title: Box stations
order: 304
description: Binding a block in the world to the mystery boxes, its particle figure and sign, and keeping it intact.
icon: map-pin
---

A box station binds a block you built to the boxes. A click on the block, or within `radius` of it, opens one box or
the box window. The block is whatever the builder put there: a chest, a barrel, a lectern or a statue. The plugin
never places it, never opens it as a container, and never lets it break.

## Adding one

Stand in front of the block and run:

| Command | The block opens |
|---|---|
| `/uxmcosmetics station add all` | The box window |
| `/uxmcosmetics station add <box>` | That box's roulette |

The console names the block: `/uxmcosmetics station add all 10 64 -3 world`. The station is written to
`stations.conf` with an id, and starts from the `station` block of [config.conf](../config/config-conf.md#station).

## What it looks like

| Part | Shipped |
|---|---|
| A figure of particles | `ELECTRIC_SPARK` in a `HALO` of radius 0.6, 0.75 above the block, every 150 ms |
| A sound for the user | `block.ender_chest.open` |
| A burst for everyone near | `TOTEM_OF_UNDYING` |
| A sign | Two lines 1.6 above the block: the box's name as a header, and `Right click to open` |

`{box}` in a line is the box's name, or `Mystery boxes` for a station of every box. A line written `@key` is a messages
line, read in each player's language. The sign's look (billboard, shadow, panel, width, scale) is
`station.hologram.appearance`, one look for every station.

## Clicks

`station.clicks` is `["right"]`. Add `"left"` for both; a player in adventure mode can left click only where the server
lets them, so check it in game.

## Changing one

`/uxmcosmetics station set <id> <piece> <value>` changes one piece; `station show <id>` prints them all. Every piece
is listed on [Operator commands](../commands/admin.md#station-set-pieces). Or edit `stations.conf` and reload.

## Protection

The block a station stands on cannot be broken by anyone, an operator in creative included. Take it away with
`/uxmcosmetics station remove <id>`; the block is an ordinary block again.

Related: [stations.conf](../config/stations-conf.md), [Mystery boxes](mystery-boxes.md)
