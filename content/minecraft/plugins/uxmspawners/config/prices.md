---
title: Prices
order: 160
description: What spawner output sells for, from your own list or a shop plugin, and what happens to items without a price.
icon: coins
---

`prices.yml`:

```yaml
price-provider: CUSTOM
dynamic-pricing: false
shop-refused-items: KEEP
price-items-by: ITEM

default-prices:
  ROTTEN_FLESH: 0.4
  BONE: 0.2
  STRING: 1.2
  ARROW: 0.3
  DIAMOND: 2
```

`price-provider` decides where a price comes from:

| Provider | Source |
|---|---|
| `CUSTOM` | `default-prices`, material by material |
| `SHOPGUIPLUS` | The sell price in ShopGUIPlus |
| `ECONOMYSHOPGUI` | The sell price in EconomyShopGUI, free or premium |
| `EXCELLENTSHOP` | The sell price in ExcellentShop's virtual shop |

With a shop provider, prices follow the shop, and `default-prices` covers the items the shop does not
have. If the shop plugin named is not installed, the console says so and `default-prices` is used.
A shop plugin whose version does not fit stops uxmSpawners at startup, naming the plugin. Changing
the provider applies with `/usp reload`.

## Settings

| Setting | Meaning |
|---|---|
| `dynamic-pricing` | `true` makes a sale from a spawner count as a sale in the shop: its sell limits and stock apply and its dynamic prices move. EconomyShopGUI and ExcellentShop only |
| `shop-refused-items` | Items the shop has but does not buy, or not for this player's rank: `KEEP` leaves them in the spawner, `PRICES` sells them at `default-prices` |
| `price-items-by` | `ITEM` prices the item itself, so a renamed or custom item is not taken for its material. `MATERIAL` prices every item by its material, as 4.2.0 did |

With `price-items-by: ITEM`, `default-prices` prices plain items only, and the shop sells an item
only if it has that very item. The items an `ITEM` spawner makes always carry a name of their own,
from `details`, so with `ITEM` they stay in the storage unsold. A server with item spawners, like
the shipped `diamond`, needs `price-items-by: MATERIAL`, or a shop that lists those exact items.

## How a price is used

| Rule | Note |
|---|---|
| Per item | The price of one item, times how many are stored |
| Multiplied | By the seller's multiplier and the spawner's Sell Multiplier boost |
| Rounded | To two decimals, and paid through Vault |

An item with no price is not sold: it stays in the storage, and the seller is told how many items
were left. Check `default-prices` against what your spawners and loot tables produce.

<Callout type="tip" title="These numbers set the economy">

Work out what one spawner makes in an hour at its `delay`, its level and your loot tables before
players have many of them: that figure, more than the price of a spawner, decides whether your
economy holds.

</Callout>

Related: [Storage & Selling](../features/storage.md#selling), [Integrations](../integrations.md)
