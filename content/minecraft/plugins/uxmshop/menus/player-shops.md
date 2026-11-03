---
title: Player shop windows
order: 403
description: The list of shops and one shop, for visitors and owners.
icon: store
---

## Shops

`/myshop list`. Every shop on the server in 10 to 16, 19 to 25 and 28 to 34 (`shop:all-shops`); a click opens it
(`shop:visit`).

## A shop

One window serves a visitor and an owner; the owner's tiles are drawn behind a condition.

| Slot | Item | Who |
|---|---|---|
| 10 to 34 | Offers: left buys (`shop:offer-buy`), right sells (`shop:offer-sell`) | Everybody |
| 47 | Selling to players on or off (`shop:toggle-sells`) | Owner |
| 48 | Buying from players on or off (`shop:toggle-buys`) | Owner |
| 50 | Telling the owner about trades (`shop:toggle-tells`) | Owner |
| 51 | Runs `myshop profits` | Owner |

Conditions: `shop:owner`, `shop:staff` (owns it or works in it). Actions for your own buttons: `shop:restock:<n>`,
`shop:withdraw:<n>`, `shop:drop-offer`. Facts: `shop-buy`, `shop-sell`, `shop-stock`, `shop-sells`, `shop-buys`,
`shop-tells`.

Related: [Player shops](../features/player-shops.md)
