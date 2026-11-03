---
title: Overview
order: 801
description: Every plugin uxmSimpleClaims reads, LuckPerms contexts and web maps.
icon: plug
---

Nothing is required.

| Plugin | Gives |
|---|---|
| Vault, VaultUnlocked, PlayerPoints, EcoBits, Treasury | The `economy.currency` |
| PlaceholderAPI | The [placeholders](../placeholders/reference.md) |
| LuckPerms | Contexts, below |
| WorldGuard | Claims never cover a region, except those in `worldguard-ignore` |
| BlueMap, Dynmap, Pl3xMap, squaremap | Web maps, below |
| Floodgate, Geyser | Forms for Bedrock players |

## LuckPerms contexts

With `integrations.luckperms-contexts`, each `true` or `false` where the player stands: `in-own-claim`,
`in-claim-member`, `in-wilderness`. For example:

```
/lp group default permission set essentials.fly true in-own-claim=true
```

## Web maps

With `integrations.web-maps`, claims are drawn on BlueMap, Dynmap, Pl3xMap and squaremap when installed, in each
claim's colour, every `integrations.web-map-seconds` (60). A claim for sale says its price.

Related: [config.conf](../config/config-conf.md#integrations)
