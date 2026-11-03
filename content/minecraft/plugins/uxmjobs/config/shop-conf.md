---
title: shop.conf
order: 610
description: The keys of a shop line, and the nine shipped lines.
icon: store
---

Every line sits under `lines` and carries `name`, `description`, `icon`, `order`, `cost`, and any of `job` with
`job-level`, `total-level`, `permission`, `stock`, then `actions`. Two lines with one `order` refuse the load.

| Line | Gate | Cost |
|---|---|---|
| `diamond-pickaxe` | Miner 15 | 250 |
| `bundle-of-saplings` | Woodcutter 5 | 60 |
| `sack-of-bone-meal` | Farmer 5 | 40 |
| `lucky-rod` | Fisherman 20 | 200 |
| `quiver` | Hunter 15 | 150 |
| `crate-of-quartz` | Builder 10 | 120 |
| `netherite-scrap` | Smith 40, stock 4 | 800 |
| `mending-book` | Enchanter 40, stock 2 | 1200 |
| `master-of-all-trades` | Total level 400, stock 1 | 5000 |

See [Points and the shop](../features/points-and-shop.md).

Related: [Shop window](../menus/shop.md)
