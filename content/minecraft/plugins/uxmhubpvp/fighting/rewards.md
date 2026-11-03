---
title: Rewards
order: 405
description: What a kill pays, the money lines, and what the killer is told.
icon: coins
---

Every kill pays the killer. The payout is the `rewards.money` list of [config.conf](../config/config-conf.md#rewards),
one line per balance.

```hocon
rewards {
  money = ["exp 10"]
}
```

The shipped line pays 10 experience points, so a lobby with nothing else installed still rewards its fighters. An
empty list, `money = []`, pays nothing.

## The money lines

| Line | Pays into | Needs |
|---|---|---|
| `exp 10` | The player's experience points | nothing |
| `levels 1` | The player's experience levels | nothing |
| `vault 10` | The server economy | Vault and an economy plugin |
| `vault-unlocked 10` | The server economy, through the modern Vault | VaultUnlocked |
| `playerpoints 2` | Points | PlayerPoints |
| `ecobits:gems 1` | One EcoBits currency, named after the colon | EcoBits |
| `treasury 10`, `treasury:coins 10` | The Treasury economy, its default currency or the one named | Treasury |

A list may mix them: `money = ["exp 10", "vault 25"]`. Amounts may have decimals where the economy takes them. A line
that cannot be read stops the reload and names the setting.

A line naming an economy the server does not run pays nothing, and says so once in the console at the start.

## What the killer is told

| Case | Moment |
|---|---|
| Every line paid | `kill`, then `kill-paid` with `<coins>`, the lines as written: *You earned 10 exp, 25 vault.* |
| A line could not pay | `kill` only. The lines that could pay still paid |

To pay more than money, such as a crate key, add a `[console]` line to the `kill` moment:

```hocon
kill = [
  "[message] @reward.kill",
  "[sound] entity.player.levelup 1.0 1.0",
  "[console] crates key give %player_name% pvp 1"
]
```

See [Moments](moments.md).

## Economy settings

```hocon
economy {
  treasury-wait-ms = 250
}
```

`treasury-wait-ms` is how long one Treasury call may take before the plugin gives up and pays nothing. It runs on the
player's own thread, so a large number is a stalled tick. Every other economy answers at once and ignores it. See
[Economies](../integrations/economies.md).

Related: [Deaths and kills](deaths-and-kills.md), [Moments](moments.md), [Economies](../integrations/economies.md)
