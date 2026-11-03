---
title: Rewards
order: 304
description: What a crown and a kill pay, the money lines, reward commands, and what the player is told.
icon: coins
---

uxmKotl pays twice: once for every crown taken and once for every kill. Both are set in the `rewards` block of
[config.conf](../config/config-conf.md#rewards) and apply to every arena. Nothing is paid by default, so an arena
pays only after you write something here.

```hocon
rewards {
  claim {
    money    = ["vault 10"]
    commands = []
  }
  kill {
    money    = ["exp 25"]
    commands = ["crates key give %player% kotl 1"]
  }
}
```

| Block | Paid when | Paid to |
|---|---|---|
| `claim` | A player takes the crown, from a free plate or from a king whose protection ran out | The new king |
| `kill` | A player in an arena dies to another player in the same arena ([Fights](fights.md#kills)) | The killer |

Each block has two lists, and both run.

| Key | Meaning |
|---|---|
| `money` | Lines of an economy and an amount. Every line is paid on its own |
| `commands` | Commands run by the console, without the slash, in the order written |

## The money lines

| Line | Pays into | Needs |
|---|---|---|
| `exp 250` | The player's experience points | nothing |
| `levels 2` | The player's experience levels | nothing |
| `vault 250` | The server economy | Vault and an economy plugin |
| `vault-unlocked 250` | The server economy, through the modern Vault | VaultUnlocked |
| `playerpoints 5` | Points | PlayerPoints |
| `ecobits:gems 3` | One EcoBits currency, named after the colon | EcoBits |
| `treasury 250`, `treasury:coins 250` | The Treasury economy, its default currency or the one named | Treasury |

A block may mix them: `money = ["exp 100", "vault 500"]`. Amounts may have decimals where the economy takes them.
The money is paid off the main thread, so a slow economy never holds up the crown changing hands.

A line naming an economy the server does not run pays nothing, and says so once in the console at the start.

## The commands

The player's name is filled into `%player%`, `<player>`, `{player}` and `%player_name%`. A name an account could not
have, such as one with a space, is never filled in: that command is skipped and the console says so, so a name can
never add an argument to a command.

```hocon
kill {
  commands = [
    "give %player% golden_apple 1",
    "lp user %player% permission settemp kotl.badge true 1d"
  ]
}
```

The commands run on the server's main thread.

## What the player is told

Every money line that pays runs the `paid` [moment](moments.md) for the player, with `<amount>` as the amount and the
economy, such as `10.00 vault`. The shipped `paid` moment says *You earned 10.00 vault.* A line that could not pay
says nothing; a command says nothing of its own. To announce a reward differently, change the `paid` moment or add
lines to `crowned` and `knocked-off`.

## Economy settings

```hocon
economy {
  treasury-wait-ms = 250
}
```

`treasury-wait-ms` is how long one Treasury call may take before the plugin gives up and pays nothing. Raise it only
for an economy on a slow database. Every other economy answers at once and ignores it. See
[Economies](../integrations/economies.md).

## Notes

- **Every claim pays, and a free plate is claimed at once.** A king who steps off the plate and back on claims a
  free plate again, and is paid again. The claim cooldown guards only a sitting king against others, so it does not
  slow this down. Keep the claim reward small, or pay for kills, which need another player.
- **A reload reads the rewards again.** Edit, `/uxmkotl reload`, and the next claim pays the new amounts.

Related: [Economies](../integrations/economies.md), [Moments](moments.md), [config.conf](../config/config-conf.md#rewards)
