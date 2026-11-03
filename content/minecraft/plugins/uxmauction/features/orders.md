---
title: Buy orders
order: 403
description: Asking the house to buy, matching strictness, filling by itself or by hand, and holding the money.
icon: shopping-cart
---

`/auction order <price> <amount> [-t time] [-c currency] [-m match] [-x]` asks the house to buy the item in hand for you,
and holds the money until it does. Without a price it opens the [order window](../menus/order.md).

| Match | A listing fills the order when it holds |
|---|---|
| `material` | Any item of that type |
| `enchanted` | The same type with the same enchantments |
| `exact` | The same item in every detail |

## Filling

| Way | How |
|---|---|
| By itself | A new listing at or below the order's price is offered to the best paying orders first, `house.work.auto-fill-candidates` (20) of them, and sells into them at once |
| By a seller | `/auction fill <order> [amount]` sells the item in hand into an order |
| Manual | An order made with `-x`, or auto off in the window, waits for a seller to fill it by hand |

A listing that sells into an order on the spot plays the `listed-and-sold` moment. What an order bought waits in the
buyer's mailbox.

`house.max-orders-per-buyer` limits open orders per player; 0 is no limit. `/auction cancel <id>` gives the money of an
order back.

Related: [Order window](../menus/order.md), [Orders window](../menus/mine.md#orders)
