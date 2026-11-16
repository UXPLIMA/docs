---
title: Item Signing
order: 144
description: Why a spawner item made in creative mode does not work, what is signed, and the items from before 5.0.0.
icon: shield-check
---

A spawner item says what it spawns. Without a check, anyone in creative mode or with an NBT editor
could write a blaze spawner, or a pickaxe with a thousand uses. uxmSpawners signs every item it
makes with a key only the server knows, and refuses an item whose signature does not match.

## What is signed

| Item | Signed data |
|---|---|
| Spawner items | The spawner type, its mob or item, and its mode |
| The spawner pickaxe | Its uses left, signed again after every use |
| Boost items | The boost they apply |
| Vanilla spawner items dropped by `on-break: VANILLA` | The mob they place |

An item that fails the check cannot be placed, does not count as the pickaxe, and does not apply its
boost. The player is told the item was not made by the server, and placing one is written to the log
file in `logs/`.

A copy of a real item, like a creative middle click or a dupe glitch in another plugin, carries the
real signature and still works. Signing stops forged items, not copied ones.

## The key

The key is made on the first start and kept in the database, with a copy in `item-signing-key.txt` in
the plugin folder. A database without a key, after a switch to MySQL or a deleted `database.db`, takes
the copy, so the items handed out keep working. A database that already holds another key wins, with
a warning in the console.

Each server makes its own key, so an item carried to another server is refused there. To share one
key between servers, copy `item-signing-key.txt` into the other server's plugin folder before its
first start. Keep the file private: with it, anyone can sign items.

## Plugin items stay plugin items

Spawner items, spawner pickaxes and boost items are used for nothing else. They cannot go into a
crafting grid, furnace, brewing stand, anvil, grindstone, smithing table or other workstation, be
traded to a villager, fed to an animal, picked up by a mob or shot from a dispenser.

## Settings

`general-settings.yml`:

```yaml
item-signing:
  enabled: true
  accept-old-items: true
```

| Setting | Meaning |
|---|---|
| `enabled` | `false` accepts every item, signed or not |
| `accept-old-items` | Items from before 5.0.0 carry no signature. While `true` they work, and the ones in a player's inventory and ender chest are converted and signed when they join |

Anyone in creative mode can make an item in the old format, so set `accept-old-items` to `false` once
the old items have gone around. Old items left in chests stop working then.

Related: [Upgrading to 5.0.0](../getting-started/upgrading.md), [Breaking Rules](breaking.md#the-spawner-pickaxe)
