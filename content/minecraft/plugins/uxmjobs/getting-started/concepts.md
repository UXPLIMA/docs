---
title: Concepts
order: 203
description: Jobs, price rows, the three currencies, levels, the payment order and the protections.
icon: lightbulb
---

## A job is a price table

A job is a file under `jobs/`. Its heart is `actions`: a behaviour (`mine`, `kill`, `craft`), then the things it is
aimed at (`diamond_ore`, `zombie`, `anvil`), then what each pays:

```hocon
actions {
  mine {
    diamond_ore { money = "22 + %level% * 0.3", experience = "26", points = "1", soft-limit = 70 }
  }
}
```

The plugin fires on the event and looks the row up. A job with no row for that work pays nothing for it.

## Three currencies

| Currency | Is | Goes to |
|---|---|---|
| `money` | The server's money | The economy plugin, through Vault and the others |
| `experience` | Job experience, which raises the job's level | The job |
| `points` | Job points | A balance spent only in the [shop](../features/points-and-shop.md) |

Vanilla experience is never a wage. It can be a cost: see [Economy](../features/economy.md).

## Levels

Each job has a curve: the experience one level costs grows linearly or geometrically, up to `maximum-level`. A level
cap holds a player below that until a node lifts it. Every price may read `%level%`, so a wage can grow with the level.

## The payment order

A wage passes one order, every time: the placed block ledger, the expression, six multipliers (decay, the protection
grade, boosters, the zone, the claim, the contract), the job's bound, the income cap, then the tax. `/jobs why` prints
it for the last piece of work.

## Safe by default

The ledger stops place and break loops, decay stops the six hour seam, grades pay spawner and farm mobs a tenth, and the
income cap is the backstop for the exploit nobody thought of. Every one is on out of the box.

## Leaving costs something

A player holds `slots.default` (3) jobs. Leaving costs money and puts the job's experience in an archive; returning
gives part of it back, less each time.

Related: [Jobs and prices](../features/jobs-and-prices.md), [Payments](../features/payments.md)
