---
title: Economies
order: 1002
description: The currencies a price can use, the number format, and duplicate refunds.
icon: wallet
---

`economy.currency` in [config.conf](../config/config-conf.md#economy) names one balance for every price, box and
refund.

| Value | Pays in |
|---|---|
| `exp` | The player's experience points. Every server has it. The default |
| `levels` | The player's experience levels |
| `vault` | The server economy, through Vault |
| `vault-unlocked` | The modern Vault |
| `playerpoints` | PlayerPoints |
| `ecobits:<currency>` | One EcoBits currency |
| `treasury`, `treasury:<currency>` | Treasury, its primary currency or one by name |

A currency whose plugin the server does not run is named once in the console at start, and nothing can be bought
(`buy.no-economy`, `box.no-economy`) until it is changed or installed.

`economy.format` writes an amount: `"#,##0.## 'exp'"` writes `1,250 exp`; `"'$'#,##0"` writes `$1,250`. Change the
prices in the family files and `boxes.conf` to fit the new currency.

`economy.treasury-wait-ms` (250) is how long one Treasury call may take; it runs on the player's thread.

Related: [Getting cosmetics](../features/getting-cosmetics.md)
