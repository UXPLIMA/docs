---
title: Output
order: 304
description: The three destinations, the storage window, price sources, banking and smelting.
icon: package
---

The owner picks one destination per minion with the hopper on the panel. You decide which of the three exist on this
server.

```hocon
destinations {
  internal { enabled = true, permission = "" }
  sell     { enabled = true, permission = "uxmminions.destination.sell" }
  chest    { enabled = true, permission = "", max-distance = 5 }
}
```

| Destination | Output goes |
|---|---|
| `internal` | Into the minion's own storage, kept in the database; a full minion stops. This brings a player back to the island |
| `sell` | Sold at once, the money credited to the owner. The shipped node makes it a rank perk |
| `chest` | Into a container within `max-distance` blocks. The container is a handle, never the truth: break it, replace it or let its chunk unload, and no item is lost |

A destination switched off, or one the owner lacks the node for, is skipped by the cycle button.

## The storage window

A live view over the database: every take is its own transaction and nothing is written back when the window closes, so
nothing duplicates while the minion works. See [Storage](../menus/storage.md). A material can be stopped there, so the
minion stops collecting it and everything else carries on.

## Prices

```hocon
prices {
  sources = ["uxmshop", "file"]
}
```

The first source that is installed and can price the item wins. `uxmshop` is a link to uxmShop, skipped without error
when it is not installed; `file` is `prices.conf`, about 100 materials from cobblestone at 0.4 to netherite scrap at
280. An item no source prices stays in the minion: never deleted, never dropped, never sold for nothing.

## Banking

```hocon
banking {
  enabled = false
}
```

On, money is held in the minion until the owner clicks the gold nugget on the panel to collect it. It is a reason to log
in and never a cap: the minion keeps working. Off, a sale credits the owner at once, except when the owner is not on the
server: then it waits in the minion and is collected next time. `effects.sold` runs when money reaches the player.

## Smelting

A level with `smelt = true` stores the cooked form, using the server's own furnace recipes, read once at start. An
override names one material and what it becomes, or `none`:

```hocon
smelting {
  overrides {
    COBBLESTONE    = STONE
    ANCIENT_DEBRIS = none
  }
}
```

A material with no recipe and no override is stored as it was produced.

Related: [Storage](../menus/storage.md), [prices.conf](../config/prices-conf.md)
