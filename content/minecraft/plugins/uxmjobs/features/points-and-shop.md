---
title: Points and the shop
order: 310
description: Job points, the shop lines, gating by job level, total level and node, and buying.
icon: store
---

Points are earned only from work (the `points` of a price row, and `[give-money] points <amount>` in a reward) and
spent only in the shop. They never touch the economy, so a reward can be priced without asking what it does to the
price of diamonds.

## shop.conf

```hocon
lines {
  diamond-pickaxe {
    name        = "@content.shop.lines.diamond-pickaxe.name"
    description = "@content.shop.lines.diamond-pickaxe.description"
    icon        = "diamond_pickaxe"
    order       = 10
    cost        = 250
    job         = "miner"
    job-level   = 15
    actions = [
      "[console] give %player_name% diamond_pickaxe 1"
      "[message] @shop.done.pickaxe"
    ]
  }
}
```

| Key | Meaning |
|---|---|
| `name`, `description` | Per language, as a job's |
| `icon` | The material the window draws |
| `order` | Where it sits; two lines with the same order refuse the load |
| `cost` | Points |
| `job`, `job-level` | The job and level that gate it |
| `total-level` | The total level across every job that gates it |
| `permission` | A node that gates it |
| `stock` | How many times one player may buy it; read but not enforced in this build |
| `actions` | What runs when it is bought, in order |

A line a player has not reached is hidden, not greyed out: a shop full of things you cannot buy reads as a paywall.

## Buying

A click in the [shop window](../menus/shop.md), or `/jobs buy <entry>` with the line's id. `%uxmjobs_points%` and
`/jobs points` show the balance; `/uxmjobs points <player> <amount>` adds or takes.

The shipped lines: a diamond pickaxe (miner 15, 250), saplings (woodcutter 5, 60), bone meal (farmer 5, 40), a lucky rod
(fisherman 20, 200), a quiver (hunter 15, 150), quartz (builder 10, 120), netherite scrap (smith 40, 800), a mending book
(enchanter 40, 1200, broadcast) and a nether star at total level 400 for 5000.

Related: [shop.conf](../config/shop-conf.md)
