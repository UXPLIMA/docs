---
title: Overview
order: 801
description: The economies, PlaceholderAPI, mcMMO, Floodgate, Geyser and claim plugins.
icon: plug
---

Nothing is required; each is a soft dependency.

| Plugin | Gives |
|---|---|
| Vault | The `vault` currency |
| VaultUnlocked | The `vaultunlocked` currency, which keeps decimals |
| PlayerPoints | The `playerpoints` currency |
| EcoBits | The `ecobits` currency |
| Treasury | The `treasury` currency |
| Any other economy | A `bridge` or `placeholder` currency |
| PlaceholderAPI | The placeholders, conditions, the `placeholder` currency |
| mcMMO | Skill levels in conditions |
| Floodgate, Geyser | Forms for Bedrock players |
| uxmQuests | Reads this plugin's trades for its `shop-buy` and `shop-sell` tasks |

Claim plugins need no hook: a shop opens only behind a container the player was allowed to open.

Related: [Currencies](../features/currencies.md)
