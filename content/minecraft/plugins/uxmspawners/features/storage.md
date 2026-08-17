---
title: Storage & Selling
order: 32
description: Virtual and XP storage, capacity tiers and selling from the menu.
icon: archive
---

## Virtual storage

Each spawner holds what it produces. A player opens the spawner, sees the stack of each item and
either withdraws it or sells it on the spot. Nothing lies on the ground, so a busy farm costs the
server one block entity rather than three hundred item entities.

```yaml
virtual-storage:
  default: true
  force-default: false
  permission: uxmspawners.settings.togglestorage
```

Turning storage off for a spawner makes it drop items into the world instead, which is what you
want for the ITEM-mode spawners you actually want players to walk through.

## XP storage

XP is kept the same way and withdrawn from the menu.

```yaml
xp-storage:
  default: true
  force-default: false
  permission: uxmspawners.settings.togglexp
```

`disable-xp-drops` in `config.yml` stops spawner mobs dropping XP on the floor at all.

## Limits

`modules/storage_limits.yml` caps how much a spawner can hold, per item and for XP, with
permission tiers on top:

```yaml
storage-limit:
  enabled: true
  default: 2000
  xpLimit: 10000
  permissions:
    uxmspawners-storage1: 5000
    uxmspawners-storage2: 10000
    uxmspawners-storage3: 20000
  xpPermissions:
    uxmspawners-xpstorage1: 15000
    uxmspawners-xpstorage2: 20000
    uxmspawners-xpstorage3: 25000
```

Keep the lists in ascending order. A player holding several of these gets the highest.

## Selling

The sell button is per spawner type (`spawnerInfo.sell_button`) and prices come from
`itemPrices.priceProvider`:

| Provider | Where prices come from |
|---|---|
| `CUSTOM` | The `itemPrices` list in `config.yml` |
| `SHOPGUIPLUS` | ShopGUIPlus |
| `ECONOMYSHOPGUI` | EconomyShopGUI, free or premium |
| `EXCELLENTSHOP` | ExcellentShop |

Money is paid through Vault. `modules/multipliers.yml` adds permission-based sell multipliers:

```yaml
multipliers:
  enabled: true
  permissions:
    uxmspawners-1-25x: 1.25
    uxmspawners-1-50x: 1.50
```

Ascending order again, highest wins. `modules/other.yml → title-messages` decides whether the
money and XP amounts are shown as a title on screen.

<Callout type="note" title="A price provider is a contract with another plugin">

With a shop provider selected, uxmSpawners does not keep its own prices — change a price in the
shop plugin and spawners follow. If the shop plugin has no price for an item, it cannot be sold
from a spawner either.

</Callout>
