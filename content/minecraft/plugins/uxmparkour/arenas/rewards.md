---
title: Rewards
order: 305
description: Milestones, the economies a milestone pays into, broadcasts, and what a player is told.
icon: coins
---

A milestone is a jump count that pays. When a run reaches it, the money lines are paid, the `milestone` moment
plays, and, if the milestone says so, the rest of the server hears about it. Milestones live in the `milestones`
block of [`config.conf`](../config/config-conf.md#milestones) and apply to every arena.

## The shipped ladder

```hocon
milestones {
  "25"  { money = ["exp 25"],  broadcast = false }
  "50"  { money = ["exp 60"],  broadcast = false }
  "100" { money = ["exp 150"], broadcast = true  }
  "250" { money = ["exp 500"], broadcast = true  }
}
```

| Jumps | Pays | Told to the server |
|---|---|---|
| 25 | 25 experience points | no |
| 50 | 60 experience points | no |
| 100 | 150 experience points | yes |
| 250 | 500 experience points | yes |

It pays experience, which every server has, so a lobby with nothing else installed still rewards its runners.

## The keys

| Key | Meaning |
|---|---|
| `"<jumps>"` | The jump count that fires the milestone, as the block's name. Two blocks on the same count stop the reload. |
| `money` | A list of lines, each an economy and an amount. Every line is paid. An empty list pays nothing and still plays the moment. |
| `broadcast` | `true` plays the `broadcast` moment for the whole server as well |

A milestone fires once per run, on the jump that reaches its count exactly. It fires again in the next run.

## The money lines

| Line | Pays into | Needs |
|---|---|---|
| `exp 25` | The player's experience points | nothing |
| `levels 1` | The player's experience levels | nothing |
| `vault 250` | The server economy | Vault and an economy plugin |
| `vault-unlocked 250` | The server economy, through the modern Vault | VaultUnlocked |
| `playerpoints 5` | Points | PlayerPoints |
| `ecobits:gems 3` | One EcoBits currency, named after the colon | EcoBits |
| `treasury 250`, `treasury:coins 250` | The Treasury economy, its default currency or the one named | Treasury |

A milestone may mix them: `money = ["exp 100", "vault 500"]`. Amounts may have decimals where the economy takes
them.

A line naming an economy the server does not run pays nothing, and says so once in the console at the start. The
milestone is still reached and the sound still plays.

## What the player is told

| Case | Line |
|---|---|
| Every line paid | The `milestone` moment, whose `@milestone.reached` line shows the jumps and `<reward>`, the lines as written (`25 exp, 500 vault`) |
| A line could not pay | `milestone.no-economy` instead of the moment: the milestone was reached and nothing was paid |
| `broadcast = true` | The `broadcast` moment as well, whose `@milestone.broadcast` line names the player and the jumps |

If one line of a milestone cannot pay, the player is told nothing was paid, even when the other lines paid. Keep
each milestone to economies the server runs.

## Economy settings

```hocon
economy {
  treasury-wait-ms = 250
}
```

`treasury-wait-ms` is how long one Treasury call may take before the plugin gives up and pays nothing. It runs on
the player's own thread, so a large number is a stalled tick. Every other economy answers at once and ignores it.

## Writing your own ladder

```hocon
milestones {
  "10"   { money = ["exp 10"],                     broadcast = false }
  "50"   { money = ["vault 100"],                  broadcast = false }
  "100"  { money = ["vault 250", "levels 1"],      broadcast = true  }
  "500"  { money = ["vault 2000", "playerpoints 25"], broadcast = true }
}
```

Run `/uxmparkour reload`. A milestone list is yours: an update never puts back one you removed. The `milestone` moment is one list for
every milestone, so a `[console]` line written there runs at each of them, with `<jumps>` naming which: see
[Moments](moments.md).

Related: [Moments](moments.md), [Economies](../integrations/economies.md), [config.conf](../config/config-conf.md#milestones)
