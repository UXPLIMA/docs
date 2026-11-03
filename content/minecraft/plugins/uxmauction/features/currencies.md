---
title: Currencies
order: 407
description: The nine currency types, what ships, readiness, whole numbers, paying players who are away, and how an amount is written.
icon: coins
---

A house may hold several currencies at once. A seller picks one when they list, and every price, bid, fee and refund of
that listing stays in it. Nothing is ever converted.

## Types

| Type | Is |
|---|---|
| `vault` | The Vault economy, which nearly every server has |
| `vaultunlocked` | VaultUnlocked, the modern Vault; it moves a decimal, so a large price keeps its last penny. Prefer it where installed |
| `playerpoints` | PlayerPoints |
| `ecobits` | EcoBits, which holds several currencies: name one with `currency-name` |
| `treasury` | Treasury, which holds several: name one, or leave it out for the primary; `wait-for` bounds a slow answer |
| `item` | An item in the inventory, counted in whole numbers; needs no plugin |
| `experience` | The player's experience, in `levels` or `points` |
| `placeholder` | A balance read through a placeholder and moved by two commands |
| `bridge` | Any other economy plugin, described by hand |

## Shipped

| Currency | Type | Order | On |
|---|---|---|---|
| `coins` | `vault` | 10 | yes |
| `points` | `playerpoints` | 20 | no |
| `diamonds` | `item`, `minecraft:diamond` | 30 | no |
| `experience` | `experience`, points | 40 | yes |

The lowest `order` is the first tab of a window and the currency a price with no name is in. A house installed alone
prices in experience; one with an economy prices in coins first.

## Readiness

A currency whose plugin is not installed stays in the list and answers that it is not ready. The house then refuses a
listing in it and says why. Install the plugin later and nothing has to change. Turn a currency off with
`enabled = false` rather than deleting it, so the ids in your database still mean something.

## Whole numbers

An `item` currency, and any plugin that is whole, counts whole numbers only: a price of half a diamond is refused, and the
message says why.

## Paying a player who is away

`pays-offline` says whether a seller who is away can be paid straight. Vault can, through its offline player. A plugin
that cannot leaves the money in the mailbox instead, and nothing is lost either way.

## How an amount is written

| Key | Meaning |
|---|---|
| `format` | Where the number and the name go: `{amount}`, `{currency}`. `@currency.coins` writes it in each player's language |
| `format-one` | The same, for exactly one: "1 coin", not "1 coins" |
| `decimals` | How many places a fraction may take |
| `trim` | Drop a fraction of zeros: 10, not 10.00 |
| `thousands`, `point` | The separators |
| `short`, `short-suffixes` | Write 1250 as 1.2k, with `k`, `M`, `B`, `T` |

A number with more places than `decimals` allows is rounded up: a price shown must never be under the price charged.

The `placeholder` and `bridge` types are in [currencies.conf](../config/currencies-conf.md).

Related: [currencies.conf](../config/currencies-conf.md)
