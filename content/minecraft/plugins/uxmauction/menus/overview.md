---
title: Overview
order: 401
description: The fourteen window files, who opens them, and the shared layout.
icon: layout-template
---

| Window | File | Opened by | Rows |
|---|---|---|---|
| [House](house.md) | `menus/house.conf` | `/auction` | 6 |
| [Category](category.md) | `menus/category.conf` | The bookshelf | 4 |
| [Listing](listing.md) | `menus/listing.conf` | A click on a listing | 3 |
| [Sell](sell.md) | `menus/sell.conf` | `/auction sell` | 5 |
| [Order](order.md) | `menus/order-create.conf` | `/auction order` | 5 |
| [Mine](mine.md) | `menus/mine.conf` | `/auction mine`, the book | 6 |
| [Bids](mine.md#bids) | `menus/bids.conf` | `/auction bids` | 6 |
| [Orders](mine.md#orders) | `menus/orders.conf` | `/auction orders` | 6 |
| [Mailbox](mailbox.md) | `menus/mailbox.conf` | `/auction mailbox`, the ender chest | 6 |
| [Watch](watch.md) | `menus/watch.conf` | `/auction watch` | 6 |
| [Search](search.md) | `menus/search.conf` | `/auction search`, the sign | 6 |
| [History](search.md#history) | `menus/history.conf` | `/auction history` | 6 |
| [Profile](search.md#profile) | `menus/profile.conf` | `/auction profile` | 6 |
| [Admin log](admin-log.md) | `menus/admin/log.conf` | `/uxmauction log` | 6 |

The six-row windows share one shape: rows 0 to 44 hold the list, the bottom row 45 to 53 is a bar of panes with the
previous arrow at 45, the buttons in the middle and the next arrow at 53. An empty list shows a cobweb at 22 that says
why.

A listing's tile draws the real item, with the client's own lines quieted by `item.hide-lines`.

Every window is a file; see [Menu files](../config/menu-files.md). The sounds of every window are `menu.sounds` in
`config.conf`.

Related: [Menu files](../config/menu-files.md)
