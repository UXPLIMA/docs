---
title: Concepts
order: 12
icon: lightbulb
---

## The farmer

One NPC, owned by one player, bound to one region. It has a location, an appearance, a storage, a
member list and a set of module switches. A player buys it with `/farmer buy` or receives a farmer
egg and right-clicks it into place.

`maximum-farmer-per-player` caps how many a player may own — 2 by default, raised per player with
`uxmfarmer.maxfarmer.<amount>`.

## Products

Every material the farmer can collect is a **product**, listed in `collected-materials.yml`. A
product is not a stack in a chest: it is a counter with a level, a capacity and a unit price. The
farmer adds to the counter; the player sells the counter.

A product the player has switched off is not collected at all — the crop simply does not grow,
unless `disabled-products-grow-physically` is on.

## Levels

Each product has its own level, and each level sets a `capacity`, a `price` to reach it, and
optionally its own `unit-price` and `tax-price`. Levels can also carry **requirements** — a
PlaceholderAPI condition the player must satisfy before the money is even asked for — and commands
to run when the level is reached.

Products without their own ladder fall back to `default-levels.yml`.

## Storage and selling

Collected products sit in the farmer's storage up to the current level's capacity. Past that,
`farm-manager` decides what happens: stop the crop growing, delete the overflow, or drop it on the
ground.

Selling takes the unit price from your [price source](../integrations.md#price-source-hook),
subtracts the tax, and pays the owner or the member who clicked. `uxmfarmer.taxdiscount.<percentage>`
reduces the tax for that player.

## Members and ranks

A farmer has an owner and members. Ranks are defined in `languages/<lang>/ranks.yml` with a
`weight` and a list of permissions; a member holds a rank, and the rank decides what they may do —
sell, collect, upgrade, move the farmer, change its skin, read the logs.

Ranks are cumulative by weight: a Leader (weight 500) has everything Member (weight 100) has, plus
its own list.

## Modules

Five behaviours, each switchable per farmer by the player and per server by you:

| Module | What it does |
|---|---|
| Production | Tracks output rate and income per minute, hour and day |
| Auto Harvest | Harvests and replants ripe crops |
| Auto Kill | Kills configured mobs in range |
| Auto Sell | Sells a product the moment its storage fills |
| XP Collector | Collects XP from mobs that die nearby and lets it be sold |

Each has a `permission-required` flag. When it is on, the **owner** must hold the permission —
members inherit the farmer's capabilities, not their own.
