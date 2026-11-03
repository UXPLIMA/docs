---
title: Economies
order: 802
description: The economies a reward pays into, what each needs, and what happens without one.
icon: wallet
---

A reward pays through the economies its `money` lines name, and a moment's `[give-money]` and `[take-money]` lines
use the same names. Two need no plugin; the rest are used when their plugin is installed. See
[Rewards](../arenas/rewards.md) for writing the lines.

| Line | Economy | Needs | Notes |
|---|---|---|---|
| `exp <amount>` | Experience points | nothing | Points, not levels: a level costs more points the higher it is |
| `levels <amount>` | Experience levels | nothing | Whole levels |
| `vault <amount>` | The Vault economy | Vault and an economy plugin | The economy plugin Vault hands out |
| `vault-unlocked <amount>` | The VaultUnlocked economy | VaultUnlocked | Registers beside the old Vault |
| `playerpoints <amount>` | PlayerPoints | PlayerPoints | Whole points |
| `ecobits:<currency> <amount>` | One EcoBits currency | EcoBits | The currency after the colon is required |
| `treasury <amount>`, `treasury:<currency> <amount>` | Treasury | Treasury | The currency is optional; the default is used without one |

## Without the economy

A line naming an economy the server does not run pays nothing, and the console says so once at the start. The claim
or kill still counts, and the player is not told they earned anything for that line. If the economy throws while
paying, the line pays nothing.

## Treasury

Treasury answers later rather than at once, so the plugin waits for it for at most `economy.treasury-wait-ms` (250 by
default) and pays nothing after that. Raise it only for an economy on a slow database.

Related: [Rewards](../arenas/rewards.md), [config.conf](../config/config-conf.md#economy)
