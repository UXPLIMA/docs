---
title: Player commands
order: 202
description: Every /auction subcommand, with its arguments, flags, node and what it does.
icon: user
---

Players only. Every node is given to everyone by default.

## Windows

| Command | Node | Opens |
|---|---|---|
| `/auction` | `uxmauction.use` | The [house](../menus/house.md) |
| `/auction mailbox` | `uxmauction.mailbox` | The [mailbox](../menus/mailbox.md) |
| `/auction mine` | `uxmauction.mine` | [Your listings](../menus/mine.md) |
| `/auction bids` | `uxmauction.mine` | The auctions you lead |
| `/auction orders` | `uxmauction.mine` | Your buy orders |
| `/auction watch` | `uxmauction.watch` | [Your watches](../menus/watch.md) |
| `/auction history` | `uxmauction.history` | What the item in your hand has sold for |
| `/auction search <text>` | `uxmauction.search` | The listings whose item names hold the text |
| `/auction profile [player]` | `uxmauction.profile` | A seller's shelf and earnings; yours without a name |

## Selling

| Command | Node | What it does |
|---|---|---|
| `/auction sell` | `uxmauction.sell` | Opens the [sell window](../menus/sell.md) for the item in hand |
| `/auction sell <price> [amount] [-t time] [-c currency] [-r]` | `uxmauction.sell` | Lists the item in hand at a fixed price; `amount` takes part of the stack in hand, `-r` lets buyers take part |
| `/auction auction <price> [-t time] [-c currency]` | `uxmauction.auction` | Lists the item in hand for bids, starting at the price |
| `/auction reprice <listing> <price>` | `uxmauction.reprice` | Lowers the price of your own listing |
| `/auction cancel <id>` | `uxmauction.cancel` | Takes your listing or order back, while `house.allow-cancelling` is on |

## Buying

| Command | Node | What it does |
|---|---|---|
| `/auction buy <listing> [amount]` | `uxmauction.buy` | Buys from a fixed-price listing; `amount` from a retail one |
| `/auction bid <amount> <listing>` | `uxmauction.bid` | Bids on an auction, in its currency. At least the leading bid plus the step |
| `/auction order [price amount] [-t time] [-c currency] [-m match] [-x]` | `uxmauction.order` | Without a price, the [order window](../menus/order.md); with one, holds the money and asks the house to buy the item in hand |
| `/auction fill <order> [amount]` | `uxmauction.fill` | Sells the item in hand into somebody's order |

## The mailbox

| Command | Node | What it does |
|---|---|---|
| `/auction claim <row>` | `uxmauction.claim` | Takes one row out of the mailbox; what the mailbox window runs on a click |

## Language

| Command | Node |
|---|---|
| `/auction lang` | `uxmauction.language` |
| `/auction lang set <language>` | `uxmauction.language` |
| `/auction lang reset` | `uxmauction.language` |

The windows write the ids into these commands for you. Typing `/auction bid 500` with no listing answers that no
listing was named: the house does not guess which auction you meant.

Related: [Selling](../features/selling.md), [Bidding](../features/bidding.md), [Buy orders](../features/orders.md)
