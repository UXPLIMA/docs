---
title: Experience and boosts
order: 406
description: Multipliers, rank nodes, the boost calendar, timed boosts, marked items and what land is worth.
icon: zap
---

## Multipliers

| Key in `experience` | Default | Meaning |
|---|---|---|
| `global-multiplier` | 1.0 | Applied before anything else |
| `per-skill` | `{}` | One per skill id |
| `maximum-multiplier` | 10.0 | The most the whole stack may reach |
| `cancelled-events-pay` | false | Whether an event another plugin cancelled still pays |
| `movement-pulse-ms` | 1000 | How often movement sources pay, scaled by the time that really passed |

## Rank nodes

`boosts.conf`:

```hocon
permission-multipliers {
  "uxmskills.multiplier.tier-one" = 1.25
  "uxmskills.multiplier.tier-two" = 1.5
}
permission-mode = "highest"
```

You name the node; the number is never inside it. A value below 1 is a penalty. `highest` takes the best node held;
`multiply` multiplies them all.

## The calendar

```hocon
schedule {
  weekend { days = [saturday, sunday], from = "00:00", to = "23:59", multiplier = 2.0, skills = [] }
}
```

`days` in English (empty for every day), `from` and `to` in the server's zone (a `to` earlier than `from` runs through
midnight), `multiplier`, and `skills` (empty for every one). Overlapping windows multiply. Nothing runs by default.

## Timed boosts and items

`/uxmskills boost <player> <multiplier> <minutes>` and `boostall` start timed boosts; `boostoff` ends every one.
`/uxmskills markitem <multiplier>` makes the held item a multiplier, `markitem skill <skill> <multiplier>` one for a
skill.

## Land

```hocon
claims {
  own        = 1.0
  others     = 1.0
  wilderness = 1.0
  providers  { }
  combine    = "any-land"
}
```

Which claim covers a block, and whether you are trusted there, is read through uxmLib's seam over nineteen protection
plugins: uxmclaims, lands, griefprevention, griefdefender, excellentclaims, simpleclaimsystem, rclaim, xclaim,
homestead, worldguard, towny, kingdoms, huskclaims, husktowns, factions, bentobox, residence, plotsquared,
superiorskyblock. `own`, `others` and `wilderness` multiply what a block pays. `providers { worldguard = false }` turns
one off (a restart); an unknown key is named in the console. `combine`: `any-land` counts a player trusted by any claim
covering the block, `all-land` needs every one.

Related: [boosts.conf](../config/boosts-conf.md), [Anti-farm](anti-farm.md)
