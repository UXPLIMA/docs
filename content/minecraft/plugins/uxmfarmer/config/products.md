---
title: Products and levels
order: 54
icon: package
---

`collected-materials.yml` is the list of everything a farmer can hold, and for each entry, what it
is worth and how it upgrades. `default-levels.yml` is the ladder used by products that do not bring
their own.

## A product

```yaml
collected-materials:
  CACTUS:
    sellable: true
    unit-price: 1.5
    tax-price: 0.15
    levels:
      0:
        price: 1000
        capacity: 2500
      1:
        price: 1000
        unit-price: 1.6
        tax-price: 0.14
        capacity: 10000
```

| Key | Meaning |
|---|---|
| `sellable` | Whether the product may be sold at all |
| `unit-price` | Money per unit, when the price source is `default` |
| `tax-price` | Tax fraction for this product |
| `levels` | The upgrade ladder, keyed by level number starting at 0 |

The key is a Bukkit material name. A material the farmer collects but which has no entry here has no
price and no capacity — so the first thing to check when a product will not sell is whether it is in
this file.

`sellable: false` gives a product that accumulates and can only be withdrawn, which is how you
handle something valuable enough that you do not want a price on it.

## Levels

Every level key is documented on
[Storage and Levels](../features/storage-and-levels.md#what-a-level-defines). The two that decide
the shape of your economy:

- `capacity` — how long a player can be away before their farm stalls
- `price` — what the next step costs

The default ladder runs 2,500 → 10,000 → 20,000 → 30,000 capacity while prices go
1,000 → 1,000 → 4,500 → 20,000. Capacity grows linearly and price grows steeply, which is the
correct direction: the last upgrade should be a serious purchase.

## Which ladder applies

A product with its own `levels` section uses it. A product without one falls back to
`default-levels.yml` entirely — you cannot mix, and there is no partial inheritance between the two
files.

Inside a single ladder, `unit-price` and `tax-price` *do* inherit from the last level that stated
them. This is the mechanism to make a product more profitable as it is upgraded:

```yaml
levels:
  0:
    price: 1000
    capacity: 2500     # inherits the product's unit-price
  4:
    price: 40000
    tax-price: 0.20    # from here on, 20% tax
  5:
    price: 75000
    tax-price: 0.19    # and 19% from here
```

## Editing products safely

Adding a product, or adding levels to the top of an existing ladder, is safe — reload and it is
live.

Removing levels from the middle, or removing a product players have stock in, is not. A farmer
sitting at a level that no longer exists has nowhere to fall back to. If a product must be retired,
set its `sellable: false` and leave the ladder in place until stocks have drained, then remove it.

<Callout type="tip" title="Reload does not migrate data">

`/uxmfarmer reload` re-reads the files; it does not adjust stored levels or stored amounts. To move
players off a level you are deleting, use `/uxmfarmer setproductlevel` on the affected farmers
first — `/uxmfarmer nearfarmers` and `/uxmfarmer uuidof` will find them.

</Callout>

## items.yml

Three items, all defined the same way:

```yaml
farmer-egg:
  give-even-if-has-farmer: true
  item:
    material: "VILLAGER_SPAWN_EGG"
    model-data: 0
    amount: 1
    glowing: true
    head-data: null
    display-name: "<#edaa0e><bold>Farmer Egg"
    lore:
      - " <#aeed0e>This egg gives you a farmer."
```

| Item | What it does |
|---|---|
| `farmer-egg` | Right-click to place a farmer |
| `set-product-level-item` | Right-click near a farmer to set a product to an exact level |
| `add-product-level-item` | Right-click near a farmer to add levels to a product |

The two level items use `<product>` and `<level>` in their lore, filled in when the item is created
by `/uxmfarmer give`. `give-even-if-has-farmer: false` stops eggs from reaching players who already
own a farmer, which matters if you hand them out through crates.
