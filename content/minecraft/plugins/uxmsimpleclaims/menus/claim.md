---
title: The claim window
order: 503
description: The claim window's buttons, and the flags, rules and history windows.
icon: layout-dashboard
---

`/claim` inside a claim you are in.

| Slot | Button | Action |
|---|---|---|
| 10 | Members | `claims:members` |
| 11 | Roles | `claims:roles` |
| 12 | Flags | `claims:flags` |
| 13 | Rules | `claims:rules` |
| 14 | Sub areas | `claims:areas` |
| 15 | History | `claims:audit` |
| 16 | Border | `claims:border` |
| 19 | Rename | `claims:rename` |
| 20 | Describe | `claims:describe` |
| 21 | Greeting | `claims:greeting` |
| 22 | Farewell | `claims:farewell` |
| 23 | Icon, from the item you hold | `claims:icon` |
| 24 | Colour | `claims:colour` |
| 25 | Category | `claims:category` |
| 28 | Teleport; shows owner, world, size, created | `claims:tp` |
| 29 | Set spawn | `claims:set-spawn` |
| 30 | Warp: left opens or closes, right sets the fee | `claims:warp-toggle`, `claims:warp-fee` |
| 31 | Favourite | `claims:favourite` |
| 32 | Sell | `claims:sell` |
| 33 | Extend, with when it expires | `claims:extend` |
| 34 | Leave | `claims:leave` |
| 40 | Delete | `claims:delete` |

Texts are asked for by the uxmLib text prompt.

## Flags

Every flag with its state and where it came from (the claim, the default or a lock). Left click turns it over
(`claims:flag-toggle`), right click puts the default back (`claims:flag-reset`).

## Rules

One row per target, with its mode and entries. Left click switches whitelist and blacklist (`claims:rule-mode`), right
click adds or removes the item you hold (`claims:rule-hand`), shift right click clears (`claims:rule-clear`).

## History

The audit trail of the claim: who, when and what (`audit.conf`).

Related: [Flags and rules](../features/flags-and-rules.md)
