---
title: Economy
order: 415
description: Where money wages go, what costs are taken in, the nine backends, the placeholder backend and bridges.
icon: landmark
---

A wage and a cost do not share a road.

## Wages

A money wage goes to the server's economy (Vault and the others), batched every `economy.batch-seconds` (5) and paid off
the region thread, with the [server account](income-cap-and-tax.md#server-account) in front when it is on. A server
with no economy still runs the whole plugin: the money wage is simply not paid, and the console says so once at load.
Experience and points are the plugin's own.

## Costs

A join fee, a leave cost, a contract price and every `money` requirement are taken through `economy.currency`, an order:
the first backend this server runs answers, and the console says at load which one and what it passed over.

```hocon
economy {
  currency = ["vault", "exp"]
}
```

| Name | Is |
|---|---|
| `vault`, `vault-unlocked`, `playerpoints`, `ecobits`, `treasury` | Those plugins; `ecobits:gems` names one currency |
| `placeholder:<key>` | A placeholder backend below |
| `exp` | The player's own experience points |
| `levels` | The player's own experience levels |
| `points` | Job points |
| A bridge name | A backend described below |

The shipped order charges money with Vault and the player's experience on a server without it, which is a real balance
but not the same price: read the console line and set your numbers. When nothing in the order runs, a cost is refused
and the player is told. `currency = []` makes every cost free. A job, contract or file that names a backend this server
does not run fails that file at load.

`treasury-wait-ms` (250) is how long one Treasury call may take before nothing is taken.

## The placeholder backend

For an economy with no API but a balance placeholder and a take command:

```hocon
placeholder {
  tokens {
    balance   = "%tokens_balance%"
    take      = "tokens take {player} {amount}"
    thousands = ","
  }
}
```

A file then writes `placeholder:tokens`. The balance is read first, and a take the player cannot afford is refused.

## Bridges

An economy plugin none of the names reach, described rather than compiled against:

| Key | Meaning |
|---|---|
| `plugin` | Its name in its own plugin file |
| `class` | The class the calls go to |
| `access` | `service`, `static`, `plugin` or `class` |
| `accessor` | For `static`: the no argument methods to follow, `getInstance.getApi` |
| `balance-method`, `take-method` | The methods that answer a balance and take an amount |
| `argument` | `offline-player`, `player-id` or `player-name` |
| `answer` | `boolean`, `vault-response` or `nothing` |
| `currencies` | `one`, `by-name` or `by-object`, with `currency-class` and `currency-lookup` |
| `take-negates` | `true` when a take is a give of a negative number |
| `caller` | What to call ourselves, for a plugin that asks |

Related: [economy.conf](../config/economy-conf.md), [Integrations](../integrations/overview.md)
