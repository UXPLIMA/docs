---
title: Your first arena
order: 104
description: Build an arena from nothing, open it, and run it, from a menu or by typing.
icon: flag
---

This page builds an arena called `sky` with the editor, then the same arena by typing. Both write the same
[`arenas.conf`](../config/arenas-conf.md) entry.

## Pick the place

Choose an empty volume of air: the course places blocks anywhere inside the box and removes them again. A box of
100 by 40 by 100 blocks is plenty for one runner; a busy lobby wants more. Put the start block near one end, and
face the long side.

## With the editor

1. Fly to where the first block should stand and type `/uxmparkour create sky`. The arena is written empty and
   shut in the world you stand in, and its editor opens.
2. Stand on a block where the course should begin and left click **Start block**. The block under your feet is
   taken. To mark a block you cannot stand on, right click instead, then click that block in the world.
3. Look along the way the course should run and left click **Heading**. The way you face is rounded to north,
   east, south or west.
4. Stand at one corner of the box and left click **Corner one**; at the opposite corner, **Corner two**. A right
   click lets you click the corner blocks from a distance.
5. Left click **Difficulty** and **Palette** to step through the difficulties and themes this arena gives a player
   who has chosen none.
6. Click **Open to players**. If a piece is missing it stays shut and the button says so; if the start block is
   outside the box, the reason is in chat.

## By typing

Every piece is one command. `here` takes the block under your feet, or for `heading` the way you face; the console
writes numbers instead.

```
/uxmparkour create sky
/uxmparkour set sky start here
/uxmparkour set sky heading east
/uxmparkour set sky corner1 -40 120 -40
/uxmparkour set sky corner2 160 170 40
/uxmparkour set sky difficulty normal
/uxmparkour set sky theme quartz
/uxmparkour show sky
/uxmparkour enable sky
```

After each `set`, the reply names what is still missing. `show` prints every piece, set or not, and the reason an
arena that has every piece still cannot open.

## Run it

1. Type `/parkour`. With one arena, that is enough; with several, `/parkour join sky`. A bare `/parkour` always
   joins the first open arena in `arenas.conf`.
2. Your inventory is put away and two items take its place: a clock in slot 4 restarts the run, a red dye in slot 6
   leaves it.
3. Jump from block to block. At 25 jumps the first milestone pays 25 experience points.
4. Fall, and you are back where you typed `/parkour`, with your inventory, and told your best.
5. `/parkour stats` shows your record, `/parkour top` the longest runs.

## Put it on a hologram

With PlaceholderAPI installed, any hologram plugin can show a board:

```
<head:%uxmparkour_top_best_alltime_1_head%> %uxmparkour_top_best_alltime_1_name%: %uxmparkour_top_best_alltime_1_value%
```

The [placeholder reference](../placeholders/reference.md) has every key, and what a place nobody holds answers.
