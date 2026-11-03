---
title: Economies
order: 902
description: Every currency name a cost, a requirement or a reward can use, placeholder currencies, and bridges to any economy.
icon: wallet
---

A crate's cost, a requirement, a reward's money and the reroll price all name a currency the same way.

## Names

| Name | Pays in |
|---|---|
| `vault` | The server economy, through Vault |
| `vault-unlocked` | The modern Vault, registered beside the old one |
| `playerpoints` | PlayerPoints |
| `ecobits:<currency>` | One EcoBits currency; EcoBits always needs one named |
| `treasury` | Treasury's primary currency |
| `treasury:<currency>` | One Treasury currency by name |
| `placeholder:<name>` | A [placeholder currency](#placeholder-currencies) |
| `<bridge>` | A [bridge](#bridges) |
| `exp` | The player's experience points; needs no plugin |
| `levels` | The player's experience levels; needs no plugin |

`vault`, `vault-unlocked` and `playerpoints` hold one balance each, so a currency after them is refused rather than
guessed at.

## Where they are written

| Place | Form |
|---|---|
| A crate's `cost` | `"[take-money] vault 500"` |
| A crate's `requirements` | `"money vault >= 500"`, `"money ecobits:gems >= 3"` |
| A reward's `money` | `"vault 500"`, `"exp 500"` |
| `rerolls.price.currency` | `"vault"` |

A crate that names a currency this server does not run is not loaded, and the console names the file and the key.

## Treasury

`economy.treasury-wait-ms` (250) is how long one Treasury call may take before the plugin gives up and takes nothing.
It runs on the player's thread, so a large number stalls a tick. Raise it only for an economy on a slow database.

## Placeholder currencies

Many economy plugins have no API but publish a balance placeholder and a take command. That pair is one currency:

```hocon
economy {
  placeholder {
    tokens {
      balance   = "%tokens_balance%"
      take      = "tokens take {player} {amount}"
      thousands = ","
    }
  }
}
```

| Key | Meaning |
|---|---|
| `balance` | The placeholder that answers the balance |
| `take` | The console line that takes it. `{player}`, `{uuid}` and `{amount}` are filled in |
| `thousands` | The group separator that plugin writes, removed before the number is read |

A crate writes `[take-money] placeholder:tokens 40`. A command cannot report a failure, so the balance is read first
and a take the player cannot afford is refused. Needs PlaceholderAPI.

## Bridges

An economy none of the names reach is described in `economy.bridges`, not compiled against. The key is the name a
crate writes. None of that plugin's code is touched unless it is installed.

```hocon
economy {
  bridges {
    mybank {
      plugin         = "MyBank"
      class          = "com.example.mybank.api.BankAPI"
      access         = "static"
      accessor       = "getInstance.getApi"
      balance-method = "getBalance"
      take-method    = "withdraw"
      argument       = "player-id"
      answer         = "boolean"
      currencies     = "one"
    }
  }
}
```

| Key | Meaning |
|---|---|
| `plugin` | The name in that plugin's own plugin file |
| `class` | The class the calls go to |
| `access` | `service`, `static`, `plugin` or `class` |
| `accessor` | For `static`: the no-argument methods to follow, such as `getInstance.getAPI` |
| `balance-method` | The method that answers a balance |
| `take-method` | The method that takes an amount |
| `argument` | `offline-player`, `player-id` or `player-name` |
| `answer` | `boolean`, `vault-response` or `nothing` |
| `currencies` | `one`, `by-name` or `by-object` |
| `currency-class`, `currency-lookup` | For `by-object`: the class that hands out currency objects and its static method that takes a name |
| `take-negates` | `true` when the plugin has no take and a take is a give of a negative amount |
| `caller` | What to call ourselves, for a plugin that asks who is moving the money |

Related: [economy.conf](../config/economy-conf.md), [Crates](../features/crates.md#permission-requirements-and-cost)
