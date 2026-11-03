---
title: Order
order: 406
description: "Opening a buy order with clicks: price, amount, time, currency, match and auto."
icon: shopping-cart
---

`/auction order`, node `uxmauction.order`. Five rows; the wanted item sits at 13. The price, amount, currency and time
buttons are those of the [sell window](sell.md).

| Slot | Button | Click |
|---|---|---|
| 20, 22, 24 | Price down, price, price up | As in the sell window |
| 29, 30, 31 | Fewer, amount, more | As in the sell window |
| 33 | Cobblestone, enchanted book or name tag: the match | Steps material, enchanted, exact |
| 34 | Clock: the time | 1h, 12h, 1d, or type it |
| 38 | Hopper or chest minecart: auto | Lets a listing fill it by itself, or waits for a seller |
| 40 | Emerald block | Opens the order and holds the money |
| 42 | Feather | Back to the house |

Related: [Buy orders](../features/orders.md)
