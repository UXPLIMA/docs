---
title: Jobs and prices
order: 301
description: Every key of a job file, the level curve, the level cap, costs, bounds, price rows and expressions.
icon: file-text
---

One job, one file under `jobs/`. The file name is the id; two files describing one job refuse the load.
`jobs/miner.conf` shows every key.

| Key | Default | Meaning |
|---|---|---|
| `name`, `description` | | What a player reads; see below |
| `icon` | Required | The material the windows draw |
| `curve` | | How levels are spaced; see below |
| `level-cap` | `maximum-level` | `default`, and a `node` prefix that lifts it |
| `seats` | 0 | How many players may hold the job at once, offline holders included; `0` for no limit |
| `currency` | `""` | What the join and leave costs are taken in: a backend, or `backend:currency`; empty is the order of `economy.conf` |
| `join-cost` | `"0"` | An expression |
| `leave-cost` | `"0"` | An expression, or `default` plus `nodes` |
| `bounds` | None | Per currency, the `floor` and `ceiling` one piece of work may pay |
| `requirements` | `[]` | Condition lines a player must pass to join |
| `actions` | | The price table |
| `rewards` | | What a level gives; see [Rewards and titles](rewards-and-titles.md) |

## Name and description

`"@content.jobs.miner.name"` reads a line of the messages files in each reader's language; a block
`{ default = "Miner", tr = "Madenci" }` gives a line per language with `default` as the fallback; a plain string is one
line everybody reads. The shipped files use the first.

## curve

| Key | Meaning |
|---|---|
| `shape` | `linear` or `geometric` (the default) |
| `base` | What level two costs |
| `step` | `linear`: what each level adds |
| `factor` | `geometric`: what each level multiplies the last by |
| `maximum-level` | The top, 1 to 10000 (100 by default) |

A level costs what the curve says for the level before it: `linear` costs `base + step * (level - 1)`, `geometric`
costs `base * factor^(level - 1)`. Experience past the top is kept.

## level-cap

```hocon
level-cap {
  default = 100
  node    = "uxmjobs.level-cap.miner."
}
```

A player holding `uxmjobs.level-cap.miner.150` climbs to 150; the highest number held wins, never above
`maximum-level`. Wages are worked out at the capped level.

## Costs

`join-cost` and `leave-cost` are expressions. Under `leave-cost.nodes`, a node with a cheaper expression; the cheapest
node the player holds wins, because a node is written to make leaving easier and never harder:

```hocon
leave-cost {
  default = "500 + %level% * 25"
  nodes {
    "uxmjobs.leave.discount" = "250 + %level% * 10"
  }
}
```

A leave cost reads `%level%`, `%experience%` and `%jobs_held%`.

## bounds

```hocon
bounds {
  money      { floor = -50, ceiling = 200 }
  experience { floor = 0,   ceiling = 150 }
  points     { floor = 0,   ceiling = 20 }
}
```

An expression is correct arithmetic and ruinous economics at level eighty, and nobody sees it until a week of play. The
bound is the number you can reason about without reading the curve, and `/uxmjobs preview` prints both sides of it. A
negative floor lets a row cost money.

## The price table

```hocon
actions {
  mine {
    stone       { money = "0.35 + %level% * 0.01", experience = "0.5" }
    gravel      { money = "0.10", experience = "0.2", until-level = 30 }
    ancient_debris { money = "60 + %level% * 0.5", experience = "70", points = "3", from-level = 40, soft-limit = 80 }
  }
  block_drop {
    diamond = "8.0"
  }
}
```

The first word is an [action type](action-types.md), the second the target it is aimed at. A row written as one value is
money.

| Row key | Meaning |
|---|---|
| `money`, `experience`, `points` | An expression per currency; a row may pay in one and not the others |
| `from-level` | The first level the row pays at (1) |
| `until-level` | The last level it pays at; `0` for no end |
| `soft-limit` | Past this level the wage stops rising while the level keeps going; `0` for none |

A row that pays nothing is refused at load.

## Expressions

Every price is arithmetic: numbers, `+ - * / ^`, brackets and an allow list of functions, and nothing else. A token is
written `%name%`:

| Token | Value |
|---|---|
| `%level%` | The level the row is paid at (held by `soft-limit`) |
| `%raw_level%` | The player's real level |
| `%from_level%` | The row's `from-level` |
| `%amount%` | How far a placeholder rose, for the `placeholder` action |
| `%damage_share%` | The share of the creature's health this player took |
| Any other | A PlaceholderAPI placeholder |

A token that answers nothing reads as `0`. Every expression is checked at load, so a bracket never closed refuses the
reload and names the file and the key.

Related: [Action types](action-types.md), [Payments](payments.md)
