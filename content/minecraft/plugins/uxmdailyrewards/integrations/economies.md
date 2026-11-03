---
title: Economies
order: 802
description: The economies a reward pays into, what each needs, and what happens without one.
icon: wallet
---

A day pays money with `[give-money] <economy> <amount>`, and `[take-money]` uses the same names. Two need no plugin;
the rest are used when their plugin is installed.

| Line | Economy | Needs | In the window |
|---|---|---|---|
| `[give-money] exp <amount>` | Experience points | nothing | `<amount> experience` |
| `[give-money] levels <amount>` | Experience levels | nothing | `<amount> levels` |
| `[give-money] vault <amount>` | The Vault economy | Vault and an economy plugin | `<amount> coins` |
| `[give-money] vault-unlocked <amount>` | The VaultUnlocked economy | VaultUnlocked | `<amount> coins` |
| `[give-money] playerpoints <amount>` | PlayerPoints | PlayerPoints | `<amount> points` |
| `[give-money] ecobits:<currency> <amount>` | One EcoBits currency | EcoBits | `<amount> <currency>` |
| `[give-money] treasury <amount>` | Treasury | Treasury | `<amount> coins` |

`exp` is points, not levels: a level costs more points the higher it is.

## Without the economy

A line naming an economy the server does not run pays nothing, and the console names the day once at start. The claim
still counts and the rest of the day still pays.

## Treasury

Treasury answers later rather than at once, so the plugin waits for it for at most `economy.treasury-wait-ms` (250
by default) and pays nothing after that. Raise it only for an economy on a slow database; it needs a restart.

Related: [Day files](../rewards/day-files.md), [config.conf](../config/config-conf.md#economy)
