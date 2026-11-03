---
title: Shop
order: 505
description: The decoration shop, its shelves, buying, using, and how a row reads.
icon: shopping-bag
---

`/plot shop`, or the panel's decoration button. Offers sit in slots 0 to 35; left click buys one (`plots:shop-buy`),
right click puts it on the plot the window was opened over (`plots:shop-use`).

| Slot | Shelf |
|---|---|
| 38 | `floor` |
| 39 | `wall` |
| 40 | `border` |
| 41 | `ground` |
| 42 | `hologram` |
| 43 | `tag` |
| 47 | `music` |
| 48 | `ambience` |
| 49 | `time` |
| 50 | `weather` |

A shelf button sets `list-filter:plots:shop:kind=<shelf>`.

| Token | Means |
|---|---|
| `%shop_id%` | What to type after `/plot buy` |
| `%shop_word%`, `%shop_icon%`, `%shop_kind%` | What a player reads, the block it is drawn with, its catalogue |
| `%shop_price%` | What it costs, or nothing when it is not for sale |
| `%shop_state%` | `free`, `owned`, `granted`, `for-sale` or `locked` |
| `%shop_cost%`, `%shop_standing%` | The price and the reader's standing, in their language |
| `%shop_showing%` | Which shelf is shown |

Conditions: `plots:shop-empty`, `plots:shop-buyable`, `plots:shop-usable`, `plots:shop-on-a-plot`.

Related: [Personality and shop](../features/personality-and-shop.md)
