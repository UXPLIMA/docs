---
title: Charges and cooldowns
order: 410
description: Items that spend charges and are refilled with fuel at the anvil, the banded tooltip line, and persistent cooldowns.
icon: battery
---

## Charges

```hocon
charges {
  enabled       = false
  maximum       = 500
  consume       = 1
  recharge      = 50
  fuel-material = "GLOWSTONE_DUST"
  bands = [
    { from = 50, format = "<good>Charges <value><left><body> / <value><max>" }
    { from = 20, format = "<level>Charges <value><left><body> / <value><max>" }
    { from = 0,  format = "<bad>Charges <value><left><body> / <value><max>" }
  ]
}
```

A knob for a powerful enchantment that touches balance nowhere else. It ships off, and a server that never turns it on
never sees it: nothing is written onto an item until it is on.

| Key | Meaning |
|---|---|
| `maximum` | The most charges one item holds |
| `consume` | How many one firing takes |
| `recharge` | How many one fuel item puts back, in the anvil |
| `fuel-material` | What recharges an item |

An item that has never been charged reads as full, so turning the system on mid season does not stop every sword at
once. An item without enough left does not fire. Put it in an anvil with the fuel item to top it up, never above the
maximum.

The band with the highest `from` the item's percentage reaches is drawn, whatever order you write them in. A percentage
no band covers draws nothing, which is how you write a warning that appears only near the end: delete the first two.
`<left>`, `<max>` and `<percent>` are in scope.

`%uxmenchants_charges%` and `%uxmenchants_charges_max%` give the numbers.

## Cooldowns

An effect's `cooldown` is per player and per enchantment. It lives in memory and is gone with the process, which is
what an operator expects of a five second cooldown on a sword. `cooldowns.persist = true` keeps them in the database
across a restart, for a cooldown long enough that a restart would be a way round it.

`%uxmenchants_cooldown_<id>%` gives the seconds left.

Related: [Anvil and books](anvil-and-books.md), [Placeholders](../placeholders/reference.md)
