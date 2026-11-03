---
title: Claims
order: 408
description: How land protection decides who may place and open a crate, the nineteen claim plugins, and turning one off.
icon: map
---

uxmCrates asks the claim plugins on the server two questions: may this operator place a crate here, and may this
player open the crate that stands here. When none is installed nothing happens: no error, no log line.

## The two rules

| Key | Default | Meaning |
|---|---|---|
| `claims.place-inside-others-claims` | `false` | An operator may place a crate inside a claim they do not own or are not trusted in |
| `claims.open-inside-others-claims` | `true` | A player may open a crate standing inside a claim they do not own. Turn it off only when crates are part of your land economy |

A refused placing answers `place.protected`; a refused opening `crate.protected-land`.

## The plugins

BentoBox, ExcellentClaims, Factions, GriefDefender, GriefPrevention, Homestead, HuskClaims, HuskTowns, Kingdoms,
Lands, PlotSquared, RClaim, Residence, SimpleClaimSystem, SuperiorSkyblock2, Towny, uxmClaims, WorldGuard and XClaim.
Every installed one is asked.

## Turning one off

```hocon
claims {
  providers { worldguard = false }
}
```

The key is the plugin's name in lower case: `uxmclaims`, `lands`, `griefprevention`, `griefdefender`,
`excellentclaims`, `simpleclaimsystem`, `rclaim`, `xclaim`, `homestead`, `worldguard`, `towny`, `kingdoms`,
`huskclaims`, `husktowns`, `factions`, `bentobox`, `residence`, `plotsquared`, `superiorskyblock`. An unlisted one is
on. A key that matches no plugin is named in the console.

## Overlapping claims

| `claims.combine` | A player counts as trusted when |
|---|---|
| `any-land` | Any claim covering the block says so: the most permissive reading |
| `all-land` | Every claim covering the block says so: the strictest |

An unknown value is named in the console and read as `any-land`.

`/uxmcrates doctor` names the claim plugins that answer.

Related: [config.conf](../config/config-conf.md#claims), [Crates](crates.md#placing-a-crate)
