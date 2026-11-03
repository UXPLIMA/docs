---
title: Prices and limits
order: 303
description: The currency, the four price curves, refunds, caps and limit groups.
icon: badge-dollar-sign
---

## The currency

`economy.currency`: `vault`, `vault-unlocked`, `playerpoints`, `ecobits:<currency>`, `treasury`, `exp` or `levels`.
`treasury-wait-ms` (250) bounds a Treasury call. Staff on bypass pay nothing.

## The price of a chunk

Counted across all of a player's claims.

| `curve` | Price of a chunk past the free ones |
|---|---|
| `flat` | `base` |
| `linear` | `base`, plus `step` for every chunk already held |
| `exponential` | `base` times `factor` to the power of the chunks already held |
| `tiered` | The price of the first tier whose `up-to` is not below the count |

| Key in `economy.price` | Default |
|---|---|
| `curve` | `linear` |
| `free-chunks` | 4 |
| `base` | 100 |
| `step` | 10 |
| `factor` | 1.05 |
| `maximum` | 5000 (0 for no cap) |
| `tiers` | Up to 16 at 100, up to 64 at 250, up to 256 at 500 |

`refund-percent` (50) of a chunk's price comes back when it is given up. `max-visit-fee` (1000) caps a warp fee and
`max-sale-price` (0, no cap) a sale price.

## Limits

```hocon
limits {
  default { claims = 3, chunks-per-claim = 64, total-chunks = 96, members = 10, roles = 2, sub-areas = 4 }
  vip     { claims = 5, chunks-per-claim = 128, total-chunks = 256, members = 20, roles = 4, sub-areas = 8 }
}
```

`default` is what every player may have. Another group applies to holders of `uxmsimpleclaims.limit.<group>`; a player
with several gets the highest of each. `uxmsimpleclaims.limit.unlimited` lifts every limit. `roles` counts roles an
owner adds of their own. `/claimadmin limits <player>` says what a player may have.

Related: [config.conf](../config/config-conf.md#economy)
