---
title: shop.conf
order: 643
description: "The island shop: items, prices and how prices move."
icon: store
---

The island shop: members buy with the island bank and sell into it. Prices move with what players buy and sell.

The file is `plugins/uxmSkyblock/modules/shop.conf`. Every default below is the shipped value.

## `shop`

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | false turns the island shop off. |
| `damping-factor` | `0.85` | How much of a price move sticks, above 0 and up to 1. Lower values make prices move slower, so buying a lot and selling it straight back earns nothing. |
| `default-stock-baseline` | `100` | The stock-baseline of an item that writes none. |
| `default-elasticity` | `0.5` | The elasticity of an item that writes none. |

## `shop.items`

What the shop trades. Add a block and the shop trades it; remove one and it does not. The key is the material's name.

Only base-price has to be written. It is the price of one unit in minor currency units, so 4000 is 40.00. floor-price and ceiling-price default to half and double the base price, and elasticity and stock-baseline fall back to the two settings above.

elasticity is how hard the price moves as players buy and sell: a higher number moves it further for the same volume. stock-baseline is the volume that counts as one full move.

## `shop.items.COBBLESTONE`

| Key | Default | What it does |
|---|---|---|
| `base-price` | `100` |  |
| `elasticity` | `0.3` |  |
| `stock-baseline` | `5000` |  |
| `shop.items.STONE.base-price` | `150` |  |
| `shop.items.STONE.elasticity` | `0.3` |  |
| `shop.items.STONE.stock-baseline` | `5000` |  |
| `shop.items.OAK_LOG.base-price` | `400` |  |
| `shop.items.OAK_LOG.elasticity` | `0.4` |  |
| `shop.items.OAK_LOG.stock-baseline` | `2000` |  |
| `shop.items.IRON_INGOT.base-price` | `2500` |  |
| `shop.items.IRON_INGOT.elasticity` | `0.5` |  |
| `shop.items.IRON_INGOT.stock-baseline` | `1000` |  |
| `shop.items.GOLD_INGOT.base-price` | `4000` |  |
| `shop.items.GOLD_INGOT.elasticity` | `0.5` |  |
| `shop.items.GOLD_INGOT.stock-baseline` | `800` |  |
| `shop.items.DIAMOND.base-price` | `20000` |  |
| `shop.items.DIAMOND.floor-price` | `8000` |  |
| `shop.items.DIAMOND.ceiling-price` | `60000` |  |
| `shop.items.DIAMOND.elasticity` | `0.7` |  |
| `shop.items.DIAMOND.stock-baseline` | `200` |  |
| `shop.items.EMERALD.base-price` | `15000` |  |
| `shop.items.EMERALD.elasticity` | `0.7` |  |
| `shop.items.EMERALD.stock-baseline` | `200` |  |
| `shop.items.WHEAT.base-price` | `120` |  |
| `shop.items.WHEAT.elasticity` | `0.4` |  |
| `shop.items.WHEAT.stock-baseline` | `4000` |  |
| `shop.items.SUGAR_CANE.base-price` | `90` |  |
| `shop.items.SUGAR_CANE.elasticity` | `0.4` |  |
| `shop.items.SUGAR_CANE.stock-baseline` | `6000` |  |
| `shop.items.NETHERITE_SCRAP.base-price` | `80000` |  |
| `shop.items.NETHERITE_SCRAP.floor-price` | `40000` |  |
| `shop.items.NETHERITE_SCRAP.ceiling-price` | `250000` |  |
| `shop.items.NETHERITE_SCRAP.elasticity` | `0.9` |  |
| `shop.items.NETHERITE_SCRAP.stock-baseline` | `50` |  |
