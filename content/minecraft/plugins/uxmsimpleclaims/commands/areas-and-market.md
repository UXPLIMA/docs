---
title: Sub areas, market and warps
order: 205
description: Sub areas and rent, selling and buying, and public warps.
icon: store
---

## Sub areas

| Command | Turkish | Does |
|---|---|---|
| `/claim areacreate <name>` | `bolumac` | Makes a sub area of the chunk you stand in |
| `/claim areachunk <area>` | `bolumchunk` | Adds the chunk you stand in to a sub area, or takes it out |
| `/claim areadelete <area>` | `bolumsil` | Deletes a sub area |
| `/claim areamember <area> <player> [role]` | `bolumuye` | Gives a player a role in a sub area |
| `/claim areaoffer <area> <price> [days]` | `bolumkira` | Offers a sub area for rent, up to `rent.max-period-days` |
| `/claim areaflag <area> <flag>` | `bolumayar` | Changes a flag inside a sub area |
| `/claim rent` | `kirala` | Rents the sub area you stand in |
| `/claim unrent` | `kirabitir` | Stops renewing the sub area you rent here |

## Market

| Command | Turkish | Does |
|---|---|---|
| `/claim sell <price>` | `sat` | Puts the claim up for sale, up to `economy.max-sale-price` |
| `/claim buy` | `satinal` | Buys the claim you stand in |
| `/claim market` | `pazar` | Opens the [market](../menus/claims.md#market) of claims and sub areas |

## Warps

| Command | Turkish | Does |
|---|---|---|
| `/claim warp <state> [fee]` | `ziyaret` | `open` or `close` the claim to visitors; the fee goes to the owner, up to `economy.max-visit-fee` |

Related: [Sub areas and rent](../features/sub-areas.md), [Market and warps](../features/market-and-warps.md)
