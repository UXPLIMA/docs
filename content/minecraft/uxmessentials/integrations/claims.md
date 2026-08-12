---
title: Land Claims
order: 1520
description: How land-claim plugins gate homes, the RTP search and poses.
---

If your server runs a land-claim plugin, uxmEssentials respects it. Homes are not set inside somebody
else's claim, `/rtp` does not land in claimed ground, and sitting down is gated the way building is.

You do not have to configure anything. Whatever supported claim plugin is installed is
detected on enable and consulted from then on. With none installed, none of this does
anything and every location is treated as free.

## Supported plugins

Nineteen claim plugins are supported, and **every installed one is consulted at once**, so
a server running two of them gets both answers folded together.

| Kind | Plugins |
|---|---|
| Our own | uxmClaims |
| Claim-block plugins | Lands, GriefPrevention, GriefDefender, ExcellentClaims, SimpleClaimSystem, RClaim, XClaim, Homestead, HuskClaims |
| Region plugins | WorldGuard, Towny |
| Town / nation plugins | HuskTowns, KingdomsX, Factions (FactionsUUID and SaberFactions) |
| Island / plot plugins | BentoBox, Residence, PlotSquared, SuperiorSkyblock2 |

None of them is a hard dependency, and none of their classes is loaded on a server that
does not have them. Each provider that binds is logged once on startup as
`event=claim_provider_bound`.

### What counts as "yours"

The plugins model land differently, so each provider translates its own idea of ownership
into the same two questions: *do you own or are you trusted here*, and *are you banned
from here*.

| Plugin | Owned / trusted means |
|---|---|
| **WorldGuard** | A region you own or are a member of covers the block. The world-wide `__global__` region never counts, so wilderness stays free. |
| **Towny** | A town plot you personally own, or your town's communal plots if you are the mayor. Town residents count as trusted. |
| **HuskClaims** | The claim you own, or any trust level the claim grants you. An admin claim has no owner. |
| **HuskTowns** | Land belongs to the town, not to a player, so any member of the town that holds the claim counts as trusted. |
| **KingdomsX** | Land belongs to the kingdom: your kingdom holding it is what counts, and a player with no kingdom never does. |
| **Factions** | Territory belongs to the faction, so your faction holding the chunk is what counts. Wilderness, safezones and warzones stay free ground. |
| **BentoBox** | The island you own; island members count as trusted. |
| **Residence** | A residence whose owner name is yours; anyone the residence grants the build flag counts as trusted. |
| **PlotSquared** | A plot you own; anyone added to it counts as trusted. |
| **SuperiorSkyblock2** | The island you lead; island members count as trusted. |
| **Others** | Their own owner and trust lists, read straight from the plugin. |

<Callout type="note" title="GriefPrevention admin claims are foreign">

A GriefPrevention **admin claim** has no owner, so it is treated as somebody else's
land: players cannot `/sethome` inside one.

</Callout>

## What actually uses claims

Three modules ask, each behind its own toggle, so you can honour claims for one and not
the others.

### Homes

`modules/homes/config.conf`:

```hocon
claims {
  enabled = true                    # honour the active claim plugin; off ignores claims entirely
  require-claim = false             # a home may ONLY be placed inside a claim you trust
  block-foreign-claims = true       # a home may not be placed inside another player's claim
  foreign-claim-chunk-distance = 0  # also reject placement within N chunks of a foreign claim; 0 disables
}
```

`block-foreign-claims` is the useful default: players build wherever they like but cannot
plant a home in someone else's base. `require-claim` is the strict inverse, for servers
where homes are meant to exist only on claimed land.

### Random teleport

`modules/teleport/rtp.conf`:

```hocon
respect-claims = true             # never drop a player inside a land claim
respect-worldguard = true         # never drop a player inside a WorldGuard region
```

Both are honoured independently, and the claim check runs first because it is the cheaper
in-memory lookup. `/rtp` asks the ownership-blind question here: a shared random-teleport
pool has no particular owner to run an access check for, so **any** claim is off limits,
including your own.

### Poses

`modules/poses/config.conf`:

```hocon
respect-claims = true
```

With it on, `/sit`, `/lay` and the rest require build or interact permission where the
player is standing, so a visitor cannot park themselves inside your base.

## Turning a provider off

Each plugin has its own switch and every one defaults **on**, so you only need a line to
turn one off. In the root `config.conf`:

```hocon
claims {
  providers {
    # uxmclaims = false
    # lands = false
    # griefprevention = false
    # griefdefender = false
    # excellentclaims = false
    # simpleclaimsystem = false
    # rclaim = false
    # xclaim = false
    # homestead = false
    # worldguard = false
    # towny = false
    # bentobox = false
    # residence = false
    # plotsquared = false
    # superiorskyblock = false
  }

  combine = "any-land"
}
```

A key you invent that is not on that list is flagged in the log rather than silently
toggling nothing.

### `combine`: folding two plugins into one answer

When more than one claim plugin covers the same block, `combine` decides how their
answers are merged:

| Value | Trusted / owner when |
|---|---|
| `any-land` (default) | **Any** covering claim says so. The permissive reading. |
| `all-land` | **Every** covering claim says so. The strict reading. |

A **ban in any covering claim always wins**, whichever mode you pick. Most servers run one
claim plugin and never touch this key.

## Tips and gotchas

- **No claim plugin means no restrictions.** Everything short-circuits to "allowed", which
  is the same path a server that turns every provider off takes.
- **`/rtp` blocks your own claim too.** That is deliberate: random teleport should not
  quietly become a free ride home.
- **Deleting a claim does not delete the homes inside it.** A home left in now-unclaimed
  land keeps working; nothing is cleaned up behind you.
- **Foreign-claim distance costs nothing until you use it.** `foreign-claim-chunk-distance`
  is `0` by default, which skips the proximity scan entirely.
- **Homes, `/rtp` and poses are independent.** Turning claims off for homes does not turn
  them off for random teleport.

Related: [Homes](../modules/homes.md), [Teleport & RTP](../modules/teleport.md), [Poses](../modules/poses.md), [Regions](../modules/regions.md)
