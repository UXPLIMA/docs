---
title: Ranking
order: 33
description: How the leaderboard is ordered, and what happens when players tie.
icon: list-ordered
---

Players are ranked by score, highest first. The rules for ties are in `general-settings.yml`:

```yaml
ranking:
  tiebreak: "SHARED"
  shared-style: "DENSE"
  shared-max: 0
```

## Ties

| `tiebreak` | Two players on the same score |
|---|---|
| `SHARED` | Share the rank, and both get that rank's reward |
| `MOST_ITEMS` | The one who deposited more items is ahead |
| `EARLIEST` | The one who reached the score first is ahead |

If players are still level after `MOST_ITEMS` or `EARLIEST`, the name that comes first
alphabetically is ahead, so a tie is never settled by chance. A value the plugin does not recognise
counts as `SHARED`.

## Numbering shared ranks

`shared-style` only matters with `SHARED`, and decides the rank that comes after a tie:

| `shared-style` | Three players, the first two tied |
|---|---|
| `DENSE` | 1st, 1st, 2nd |
| `COMPETITION` | 1st, 1st, 3rd |

With `COMPETITION`, nobody holds 2nd place in that example, so the 2nd place reward is not paid.

## Limiting how many can share

`shared-max` caps how many players can share one rank. With `shared-max: 2` and four players tied
for first, two share 1st and the other two move down to the next rank. `0` means no limit.

## The leaderboard

The leaderboard button in the menus shows the current run's top players, grouped by rank, so tied
players appear on one line. The `%supplyrush_rank%` [placeholder](../placeholders.md) shows a
player's own rank.

Related: [Rewards](rewards.md), [general-settings.yml](../config/general-settings.md)
