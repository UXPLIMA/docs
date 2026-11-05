---
title: Boxed
order: 307
description: Real land inside a box that every advancement pushes out.
icon: box
---

A Boxed island is played on land the server makes the usual way, inside a box. The box starts small and every
advancement one of the island's players makes pushes it out. Each advancement counts once for an island, whoever on
it makes it. Players see where their box ends as a world border, and nobody builds, breaks or walks outside it.

## The world

The `boxed` preset names the world Boxed islands are made in, `boxed` as shipped. Make that world the server's usual
way, with generated terrain, not with this plugin's empty generator.

| `land.from` | A new island plays on |
|---|---|
| `template`, as shipped | The same land on every island, copied from one place of the world, so no island starts on better land |
| `world` | The land its world made where it stands |

## Growing the box

| How | What it does |
|---|---|
| An advancement | Pushes the box out by the blocks set per advancement |
| `/is boxbuy` | Buys growth, at the price `buy` in the file asks: money, the bank, items, a level or a permission |
| `/is boxside [side]` | Chooses the side the box grows toward: every side, a side, or a corner |
| An ender pearl | Thrown from inside the box, moves the box to where it lands, as far as the box may reach from the island centre |

Every number is in [boxed.conf](../config/boxed-conf.md). Staff verbs: `/isadmin boxed setsize` and `grow`.
