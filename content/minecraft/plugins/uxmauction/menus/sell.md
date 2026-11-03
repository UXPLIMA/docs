---
title: Sell
order: 505
description: "Listing the item in hand with clicks: price, amount, time, currency, kind and retail."
icon: tag
---

`/auction sell`, node `uxmauction.sell`. Five rows; the item sits at 13.

| Slot | Button | Left | Right | Shift left | Shift right |
|---|---|---|---|---|---|
| 20 | Redstone: price down | -1 | -10 | -100 | -1000 |
| 22 | Gold ingot: the price | Next currency | Type the price in chat | | |
| 24 | Emerald: price up | +1 | +10 | +100 | +1000 |
| 29 | Arrow: fewer | -1 | | -16 | |
| 30 | Paper: the amount | Type it in chat | | | |
| 31 | Spectral arrow: more | +1 | | +16 | |
| 33 | Sunflower or gold block: the kind | Fixed price or auction | | | |
| 34 | Clock: the time | 1h | 12h | 1d | Type it in chat |
| 38 | Bundle or shulker box: retail | One lot or in parts | | | |
| 40 | Emerald block | Puts it on the shelf | | | |
| 42 | Feather | Back to the house | | | |

A typed value waits `asking.wait-for` for the answer; `cancel` stops it.

Related: [Selling](../features/selling.md)
