---
title: Rewards
order: 403
description: What a find, a milestone and completing the map pay, the money lines, and commands.
icon: coins
---

Three things pay: each find, by its type; each milestone; and finding every egg on the map. A reward is the same
shape everywhere: money lines and console commands.

```hocon
reward {
  money    = ["exp 50", "vault 10"]
  commands = ["crates key give %player% egg 1"]
}
```

| Key | Meaning |
|---|---|
| `money` | Lines of an economy and an amount. Each line is paid on its own |
| `commands` | Commands run by the console, without the slash. `%player%` is the finder's name |

## Per find

The `reward` block of the [type file](../config/type-files.md). It is paid at every first find and at every repeat
find of a repeatable type.

## Milestones

```hocon
milestones {
  first {
    at        = 5
    money     = ["exp 250"]
    commands  = []
    broadcast = false
  }
  half {
    at        = 25
    money     = ["exp 1000"]
    commands  = []
    broadcast = true
  }
}
```

| Key | Meaning |
|---|---|
| the block name | A name you choose. Only `at` matters to the plugin |
| `at` | The number of eggs found that crosses it, at least 1. Two milestones at one number stop the reload |
| `money`, `commands` | The reward |
| `broadcast` | `true` also runs `milestone-broadcast`, which tells the whole server |

A milestone is crossed by the first find that brings the player's count to `at`, and pays once per player. The
`milestone` moment runs for the finder with `<count>`. The milestones are yours: an update never puts back one you
removed.

## Completion

```hocon
completion {
  money     = ["exp 5000"]
  commands  = []
  broadcast = true
}
```

Paid when a first find brings the player's count to the number of eggs on the map. The `completion` moment runs, and
with `broadcast = true` so does `completion-broadcast`. Eggs placed later raise the total; finding them completes the
map, and pays, again.

## The money lines

| Line | Pays into | Needs |
|---|---|---|
| `exp 50` | The player's experience points | nothing |
| `levels 1` | The player's experience levels | nothing |
| `vault 250` | The server economy | Vault and an economy plugin |
| `vault-unlocked 250` | The server economy, through the modern Vault | VaultUnlocked |
| `playerpoints 5` | Points | PlayerPoints |
| `ecobits:gems 3` | One EcoBits currency, named after the colon | EcoBits |
| `treasury 250`, `treasury:coins 250` | The Treasury economy, its default currency or the one named | Treasury |

A line that cannot be read, such as a negative amount, stops the reload and names the setting. A line naming an
economy the server does not run pays nothing, and the console says so at the start and on each reload, naming the
type or milestone it belongs to. A payment the economy refuses is logged with the player's name.

The plugin says nothing about money on its own. To tell the finder what they earned, write it into the type's
effects or the milestone's message, such as *You found a Golden Egg and 500 experience*.

## Economy settings

```hocon
economy {
  treasury-wait-ms = 250
}
```

How long one Treasury call may take before the plugin gives up and pays nothing. Every other economy ignores it. See
[Economies](../integrations/economies.md).

Related: [Types](types.md), [Moments](moments.md), [config.conf](../config/config-conf.md#milestones)
