---
title: Prices
order: 60
description: What spawner output sells for, from your own list or a shop plugin, and what happens to items without a price.
icon: coins
---

```yaml
itemPrices:
  priceProvider: CUSTOM
  ROTTEN_FLESH: 0.4
  BONE: 0.2
  STRING: 1.2
  ARROW: 0.3
  DIAMOND: 2
```

in `config.yml`. `priceProvider` decides where a price comes from:

| Provider | Source |
|---|---|
| `CUSTOM` | The list under it, material by material |
| `SHOPGUIPLUS` | The sell price in ShopGUIPlus |
| `ECONOMYSHOPGUI` | The sell price in EconomyShopGUI, free or premium |
| `EXCELLENTSHOP` | The best sell price in ExcellentShop's virtual shop |

With a shop provider, the list is ignored and the shop's price changes follow automatically. If the
shop plugin named is not installed, the console says so at startup and the list is used instead.
Switching back to `CUSTOM` needs a restart; a reload keeps the shop.

## How a price is used

| Rule | Note |
|---|---|
| By material | Keys are material names. A renamed or enchanted item sells as its material |
| Per item | The price of one item, times how many are stored |
| Multiplied | By the seller's multiplier from `modules/multipliers.yml` |
| Rounded | To two decimals, and paid through Vault |

An item with no price, here or in the shop, is worth 0. With a shop provider, the console and
`logs/errors.txt` note each missing price whenever a total is worked out. With `CUSTOM` nothing is
noted, so check the list against what your spawners and loot tables produce.

<Callout type="danger" title="Sell everything throws away items without a price">

The sell button empties the whole storage and pays only for what has a price. Give every item your
spawners produce a price, or tell players to take out what they want to keep before they sell.

</Callout>

## Setting prices

These few numbers set the income of every farm on the server. Work out what one spawner makes in an
hour at its `delay`, its level and your loot tables before players have many of them: that figure,
more than the price of a spawner, decides whether your economy holds.

Related: [Storage & Selling](../features/storage.md#selling), [Integrations](../integrations.md)
