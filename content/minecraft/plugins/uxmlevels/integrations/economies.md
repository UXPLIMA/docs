---
title: Economies
order: 902
description: The economies a prestige is charged against and a reward pays into, what each needs, and what happens without one.
icon: wallet
---

`economy.currency` names the balance a prestige's `money-cost` is charged against, and a reward's
`[give-money] <economy> <amount>` uses the same names.

| Name | Economy | Needs |
|---|---|---|
| `exp` | Vanilla experience points | nothing |
| `levels` | Vanilla experience levels | nothing |
| `vault` | The Vault economy | Vault and an economy plugin |
| `vault-unlocked` | The VaultUnlocked economy | VaultUnlocked |
| `playerpoints` | PlayerPoints | PlayerPoints |
| `ecobits:<currency>` | One EcoBits currency | EcoBits; the currency is required |
| `treasury`, `treasury:<currency>` | Treasury | Treasury |

With a track on the experience bar, `exp` and `levels` are the bar this plugin paints; see
[Heads-up](../tracks/heads-up.md). Name another economy for a money cost on such a server.

## Without the economy

A currency the server does not run is named in the console once at start, and a prestige with a money cost then
answers `prestige.no-economy` for everybody until it is changed. A `money-cost` of `0` asks nothing of an economy.

## Treasury

Treasury answers later rather than at once, so the plugin waits at most `economy.treasury-wait-ms` (250) and charges
nothing after that.

Related: [Prestige](../tracks/prestige.md), [config.conf](../config/config-conf.md#economy)
