---
title: Claims
order: 308
description: Placing on claimed land, working inside the claim, the leader bypass, and what happens when the land is lost.
icon: map
---

```hocon
claims {
  respect-build-permission = true
  work-inside-claim-only   = true
  leader-bypass            = true
  on-claim-lost            = "refund"
  check-interval-seconds   = 60
  disabled-providers       = []
}
```

| Key | Meaning |
|---|---|
| `respect-build-permission` | A minion is not placed where its owner may not build |
| `work-inside-claim-only` | A minion works only inside the claim it stands in; off, its range reaches wherever it reaches |
| `leader-bypass` | The claim leader may open and use a minion on their own land without being trusted |
| `on-claim-lost` | `remove` takes the minion away; `refund` returns it to the owner as an item |
| `check-interval-seconds` | How often the land under online players' minions is checked |
| `disabled-providers` | Claim systems not to read, by name |

A minion placed inside a claim carries that claim's identity. When the claim under it is no longer that one (disbanded,
or the owner put out), `on-claim-lost` is applied. A disband is not an event the nineteen claim systems agree on, so it
is read on the interval rather than waited for.

The claim systems: WorldGuard, GriefPrevention, Lands, Towny, Factions, Residence, PlotSquared, HuskClaims, HuskTowns,
Kingdoms, BentoBox, SuperiorSkyblock2, GriefDefender, SimpleClaimSystem, ExcellentClaims, Homestead, RClaim, XClaim and
uxmClaims. With none installed nothing happens.

Related: [Limits](limits.md)
