---
title: Overview
order: 401
description: Every window, its file, its size and what opens it.
icon: layout-grid
---

| Window | File | Rows | Opened by |
|---|---|---|---|
| [Hub](catalogue.md) | `hub.conf` | 3 | `/shop` |
| [Section](catalogue.md#section) | `section.conf` | 6 | A section on the hub, `/shop open <section>` |
| [Quantity](catalogue.md#quantity) | `quantity.conf` | 3 | Buying or selling a line or an offer |
| [Sell](catalogue.md#sell) | `sell.conf` | 3 | `/sell window` |
| [Shops](player-shops.md) | `shops.conf` | 6 | `/myshop list`, the hub's ender chest |
| [A shop](player-shops.md#a-shop) | `playershop.conf` | 6 | Right clicking a shop, `/myshop` |

Every window is a file. The words are not in it: every title, button and tile line names a key of the language files,
so editing a window never touches a translation. A Bedrock player gets forms, except the sell window, which holds items
and is a chest for everybody. `chest-only = true` on an item keeps a Bedrock player on the chest window.

Related: [Menu files](../config/menu-files.md)
