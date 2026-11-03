---
title: Fuel
order: 305
description: Fuel grades, the budget in actions, what spends it, and turning it off.
icon: flame
---

Fuel ships on, because it is the only natural brake a minion economy has. `fuel.enabled = false` in `config.conf` turns
it off, and then a minion runs forever at its level's own speed.

Fuel is an item and the budget is in actions: it falls only when an action is performed, never per second or per tick.
A minion that is stopped, blocked, full or waiting spends nothing, and a minion does not work while its owner is
offline, so nothing drains overnight. One fuel item burns at a time.

## fuel.conf

| Grade | Item | Faster by | Lasts |
|---|---|---|---|
| `coal` | Coal | 10% | 32 actions |
| `charcoal` | Charcoal | 10% | 24 actions |
| `coal-block` | Coal block | 20% | 320 actions |
| `blaze-rod` | Blaze rod | 35% | 200 actions |
| `lava-bucket` | Lava bucket | 40% | 400 actions |
| `nether-star` | Nether star | 60% | 2000 actions |

`item` is the material, `speed` the percentage taken off the action time (0 to 90), `actions` how many actions one item
pays for. The [fuel window](../menus/fuel.md) offers them slowest first, whatever order you write. Delete a block and
the item stops being fuel.

A level's `fuel-per-action` is how much of the budget one action costs. A [feeder](jobs.md) refuels its neighbours from
its own stock.

`%uxmminions_minion_<id>_fuel%` gives the actions left.

Related: [fuel.conf](../config/fuel-conf.md)
