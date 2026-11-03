---
title: Economy
order: 309
description: The balance sales pay into, experience per action, the three closed loops, and outcome effects.
icon: landmark
---

## The balance

```hocon
economy {
  currency         = "exp"
  treasury-wait-ms = 250
}
```

One name for the whole plugin: what a sale pays into, and what a level, a priced button and a teleport are charged
against.

| Name | Is |
|---|---|
| `exp` | The player's own experience points; every server has it |
| `levels` | The player's own experience levels |
| `vault` | The server economy through Vault |
| `vault-unlocked` | The modern Vault |
| `playerpoints` | PlayerPoints |
| `ecobits:<currency>` | EcoBits, which always wants the currency |
| `treasury`, `treasury:<currency>` | Treasury |

The shipped `exp` makes a minion sell and pay on a bare server. Naming an economy this server does not run says so once
at start, and a seller then fills up and stops, because nothing can be paid.

## Experience per action

```hocon
experience {
  enabled      = false
  per-action   = 0
  per-material { DIAMOND = 10 }
}
```

`per-action` is the wage for turning up, granted on every action; `per-material` is a bonus per item found. The two are
added. The experience goes to the owner, who is present. A type file may write its own block over this one, key by key.
It ships off.

## The three loops

```hocon
loops {
  fuel-from-minion-output     = false
  feeder-feeds-feeder         = false
  slayer-paid-for-spawner-mob = false
}
```

| Loop | Opened, it lets |
|---|---|
| `fuel-from-minion-output` | A minion burn fuel another minion produced: a farm that pays its own upkeep |
| `feeder-feeds-feeder` | A feeder feed another feeder, forever |
| `slayer-paid-for-spawner-mob` | A slayer be paid for a mob the same owner's spawner minion produced |

All three are closed: open one only on purpose. A minion never feeds itself, and that has no key.

## Outcome effects

```hocon
effects {
  refused  = ["[sound] block.note_block.bass 1 1"]
  upgraded = ["[sound] block.anvil.use 1 1"]
  sold     = ["[sound] entity.experience_orb.pickup 1 1"]
}
```

Lists with no ceiling: `[message]`, `[broadcast]`, `[actionbar]`, `[title]`, `[subtitle]`, `[bossbar]`, `[particle]`,
`[sound]`, `[effect]`, `[console]`, `[player]`, each with `{delay=, chance=, repeat=, every=, if=}`. An empty list is
silence.

Related: [Output](output.md), [config.conf](../config/config-conf.md)
