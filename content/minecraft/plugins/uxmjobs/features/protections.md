---
title: Protections
order: 405
description: The placed block ledger, decay, farm grades, creative, fertiliser, generators, vehicles, damage share, claims and worlds.
icon: shield-check
---

Every protection is on by default: an operator who changes nothing has a safe economy. Each grades rather than bans.

## The placed block ledger

```hocon
ledger {
  enabled                = true
  retention-days         = 14
  prune-interval-minutes = 60
}
```

A block a player placed pays nothing when broken, and a spot that already paid inside the window pays nothing again.
The flag is persistent, not a timer: a stack of dirt placed an hour ago still pays nothing. `retention-days` is how long
a row is kept (0 keeps them forever); every prune is logged with how many rows it dropped.

## Decay

```hocon
decay {
  enabled          = true
  threshold        = 64
  step             = 0.01
  floor            = 0.25
  recovery-minutes = 10
  region-chunks    = 4
  retention-days   = 7
}
```

The ledger stops place and break loops; decay stops the player who mines one seam for six hours, which no ledger sees.
After `threshold` repeats of one material in one region (`region-chunks` chunks a side) by one player, each repeat
takes `step` off, down to `floor`, and one step comes back every `recovery-minutes`.

## Grades

`protections.conf`:

| Key | Default | Meaning |
|---|---|---|
| `spawner-fraction` | 0.1 | A creature from a spawner pays this share |
| `artificial-fraction` | 0.1 | A creature a player put in the world pays this share |
| `hopper-fed-fraction` | 0.2 | A workstation a hopper feeds |
| `hand-fed-bonus` | 1.25 | A workstation the player feeds by hand pays more |
| `pay-in-creative` | false | Creative mode pays nothing |
| `pay-for-fertilised` | false | A crop grown with bone meal pays nothing |
| `damage-share` | 0.5 | The share of a creature's health a player must take off before its death pays them |
| `generator-materials` | stone, cobblestone, basalt, obsidian | Never pay from a generator |
| `forbidden-vehicles` | minecart, boat, chest_boat | Nothing pays while riding one |
| `artificial-spawn-reasons` | spawner, spawner_egg, dispense_egg, trial_spawner, built_snowman, built_iron_golem, slime_split | What counts as artificial |
| `boss-mobs` | ender_dragon, wither, warden, elder_guardian | Exempt from `damage-share` |
| `disabled-worlds` | `[]` | Worlds that pay nothing |

## Claims

```hocon
claims {
  enabled                  = true
  foreign-claim-multiplier = 0.0
  own-claim-multiplier     = 1.0
  combine                  = "any-land"
  providers { }
}
```

Work inside a claim the player does not own pays `foreign-claim-multiplier` (nothing by default); a number above one
pays a bonus, for a server that rents land. Work in their own claim pays `own-claim-multiplier`. `combine` is `any-land`
or `all-land` when two claim plugins cover one block; a key under `providers` set to `false` ignores one plugin
(`uxmclaims`, `lands`, `griefprevention`, `griefdefender`, `excellentclaims`, `simpleclaimsystem`, `rclaim`, `xclaim`,
`homestead`, `worldguard`, `towny`, `kingdoms`, `huskclaims`, `husktowns`, `factions`, `bentobox`, `residence`,
`plotsquared`, `superiorskyblock`). A key not on that list is warned about in the console. With no claim plugin nothing
happens.

Related: [Payments](payments.md), [protections.conf](../config/protections-conf.md)
