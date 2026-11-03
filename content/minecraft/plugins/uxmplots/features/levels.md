---
title: Levels
order: 410
description: What raises a plot's level, the curve, and what a level unlocks.
icon: trending-up
---

`levels.conf`. Every point is something somebody else did, so a level cannot be farmed by its owner. A level is worked
out every time anybody asks and never stored.

```hocon
curve { base = 100, growth = 1.5, highest = 10 }
weights { vote = 10, visit = 1, tip = 1, contest-place = 250 }
```

Level `n` costs `base * growth^(n-1)`, and reaching it means paying for every level below; level one is free.

| Weight | Per |
|---|---|
| `vote` | Person who rated the plot |
| `visit` | Different person who visited |
| `tip` | Whole unit tipped, whatever the currency |
| `contest-place` | Place in a contest: the full weight for first, half for second, a third for third |

Zero turns one off.

## Unlocks

```hocon
unlocks {
  "3"  { extra-plots = 1 }
  "6"  { extra-plots = 2, extra-merges = 2 }
  "10" { extra-plots = 4, extra-merges = 5, catalogue = ["floor/gold-veined"], flags = ["explosions"] }
}
```

| Key | Gives |
|---|---|
| `extra-plots` | More plots for the owner, counted from their best plot |
| `extra-merges` | More plots held as one |
| `catalogue` | Entries as `kind/id`, while the best plot keeps the level |
| `flags` | Flags this plot may set even where the world locks them |

`/plot level` shows how far a plot has climbed and what raised it. `%uxmplots_plot_level%` and its `_points` and
`_next` forms give the numbers.

Related: [levels.conf](../config/levels-conf.md)
