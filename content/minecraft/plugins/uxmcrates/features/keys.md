---
title: Keys
order: 403
description: Virtual and physical keys, giving and taking them, sending, crafting, the key window and the key ledger.
icon: key-round
---

A key is a file under `keys/`; its file name is its id. A crate names the key it takes in its `key` block:

```hocon
key { id = "common-key", amount = 1, kind = "either" }
```

## Virtual and physical

| `kind` | The crate accepts | Notes |
|---|---|---|
| `virtual` | A balance in the database | Survives death and lava; granted to a player who is away; what a web store gives |
| `physical` | The key item in the inventory | Can be traded, dropped, sold and lost |
| `either` | Both | The item is spent first, then the balance |

A crate with `amount = 0`, or no `key` block, takes no key at all.

## Getting keys to players

| Way | Gives |
|---|---|
| `/uxmcrates key give <player> <key> <amount>` | A balance, online or away |
| `/uxmcrates key giveoffline <player> <key> <amount>` | The same balance, under its own node for a store |
| `/uxmcrates key giveall <key> <amount>` | A balance to every online player |
| `/uxmcrates key giveitem <player> <key> <amount>` | The item; what does not fit waits in the pending rewards |
| `/uxmcrates key set` and `key take` | Write or lower a balance |
| `/crates send <player> <key> <amount>` | Moves a player's balance to another player, when `tradeable = true` |
| Crafting | The recipe in the key file |
| A reward | `commands = ["uxmcrates key give %player% elite-key 1"]` |

## The key item

```hocon
item {
  material    = "TRIPWIRE_HOOK"
  amount      = 1
  glow        = true
  lore        = [ "@content.keys.common-key.item.lore.1" ]
  # item-model = "uxm:common_key"
  # model-data = 1001
}
```

The item carries the key's id in its own data, so renaming it does nothing and a renamed tripwire hook is not a key.
Any key the uxmLib item format reads works in this block.

## Trading

`tradeable = false` (the shipped value) refuses `/crates send` for that key. It does not stop a physical key item from
being dropped or traded by hand.

## Crafting

```hocon
crafting {
  enabled    = true
  shapeless  = false
  amount     = 1
  permission = "uxmcrates.craft.common"
  recipe = [
    "air",        "gold_ingot",    "air",
    "gold_ingot", "tripwire_hook", "gold_ingot",
    "air",        "gold_ingot",    "air"
  ]
}
```

Nine entries, read left to right and top to bottom, `air` for an empty square. `shapeless` ignores the positions.
`permission` is checked as the result appears: without it the bench shows nothing. The recipe is registered at start
and on reload.

## The key window

`/crates keys` shows each key with how many the player holds and the crates it opens. The key file's `menu` block
controls its tile:

| Key | Meaning |
|---|---|
| `order` | Where the tile sits, lowest first |
| `hidden` | `true` keeps the key off the window |
| `click.left`, `click.right` | Effect lines run when the tile is clicked, such as `"[player] shop keys"` |
| `store` | Lines shown to a player who holds none: where to get one |

## The ledger

Every grant, spend, transfer and set is a row in the key ledger, with the amount, the key, the source and the time.
`/uxmcrates key ledger <player>` reads it, newest first. `retention.ledger-days` trims old rows; `0` keeps them all.

Related: [Key files](../config/key-files.md), [Keys window](../menus/keys.md), [Operator commands](../commands/admin.md#keys)
