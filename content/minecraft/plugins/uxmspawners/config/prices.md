---
title: Prices
order: 60
description: Where sale prices come from, and the four price providers.
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

`priceProvider` decides where a sell price comes from:

| Provider | Source |
|---|---|
| `CUSTOM` | The list in this file |
| `SHOPGUIPLUS` | ShopGUIPlus |
| `ECONOMYSHOPGUI` | EconomyShopGUI, free or premium |
| `EXCELLENTSHOP` | ExcellentShop |

With anything other than `CUSTOM`, the list below is ignored entirely.

Keys are Bukkit material names and values are the price for one item. An item with no price cannot
be sold from a spawner — which is a usable way to force a material to be withdrawn and used rather
than turned into money.

Payment goes through Vault, multiplied by whatever `modules/multipliers.yml` grants the player.

<Callout type="note" title="Spawner farms are an economy, not a feature">

These few numbers set the income of every farm on the server. Work out what one spawner earns per
hour at your `delay` and `range` before opening it — that figure, not the price of the spawner,
is what decides whether your economy holds.

</Callout>
