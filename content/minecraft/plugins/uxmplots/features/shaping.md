---
title: Shaping a plot
order: 306
description: Floors, walls, borders, ground and biomes, clear and reset, snapshots, templates, merging and the work queue.
icon: paintbrush
---

## Looks

`/plot floor`, `/plot wall`, `/plot border` and `/plot ground` lay a pattern the world offers (`<face> list` names
them); `/plot biome` paints one of its biomes. Ground can bury a build, so the plot is photographed first; the bottom
course of the world is never written. A pattern is had by its node, by a [shop](personality-and-shop.md) purchase, or by
a [level](levels.md) unlock. A claim repaints the border course (the one course on top of the wall) to mark the plot taken;
an owner's own border stands over that mark, and giving the plot up puts the mark back.

## Clear and reset

`/plot clear` takes down what was built and keeps the ground. `/plot reset` puts the plot back as generated. Both ask
twice and take a picture first.

## The work queue

```hocon
work {
  blocks-per-batch = 20000
  tps-floor        = 17.0
  held             = []
}
```

A reset of a 64 block plot is about half a million blocks, so it runs a batch at a time and waits while the server is
below the floor (never below 5). Nothing is cancelled; the player who asked is told once that it is waiting (`held`).

## Snapshots

```hocon
snapshots {
  keep           = 5
  keep-automatic = 5
  before-reset   = true
  scheduled { every-minutes = 0, per-sweep = 10 }
}
```

A picture is the plot's blocks written as runs and gzipped under `snapshots/`. `keep` caps the ones somebody asked for
with `/plot snapshot`; `keep-automatic` caps the ones taken before a reset, clear or template and on the timer. The
timer is off until you write a period, and it photographs only plots built on since their last picture, `per-sweep`
at a time. `/plot snapshot restore <name>` puts one back.

## Templates

Staff keep a plot as a layout with `/plot template save [as]` (into `templates/`). An owner puts one down with
`/plot template <name>`; the plot is photographed first. A world's `starter-template` is laid on every new plot.

## Merging

```hocon
merge {
  most-plots = 4
}
```

`/plot merge <north|east|south|west>` joins your plot with your neighbour on that side, rebuilding the road between them
and, where four meet, the crossing. Both must be yours. Everything is answered by the plot you stood on: one owner, one
trust list, one set of flags. `/plot unmerge` splits it again. `most-plots = 1` turns merging off; levels add
`extra-merges`.

Related: [Plot worlds](worlds.md#patterns-and-biomes), [Levels](levels.md)
