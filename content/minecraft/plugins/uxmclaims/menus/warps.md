---
title: Warps
order: 409
description: Creating warps in a claim, and publishing them to the server.
icon: map-pin
---

Four screens: `claim_warp_list.yml`, `claim_warp_create.yml`, `claim_warp_manage.yml` and
`claim_warp_rename.yml`. Opened from claim management.

Managing warps needs `MANAGE_WARPS`. Using one needs `USE_WARPS`.

## The list

Every warp in the claim, showing its name, location, whether it is public, and who created it.
Clicking opens the management screen for that warp.

## Managing one

| Button | Does | Ability node |
|---|---|---|
| Teleport | Goes there | — |
| Move here | Relocates it to where you stand | `uxmclaims.ability.warp.relocate` |
| Rename | Prompts in chat | `uxmclaims.ability.warp.rename` |
| Public / private | Toggles visibility | `uxmclaims.ability.warp.visibility` |
| Delete | Removes it | `uxmclaims.ability.warp.delete` |

## Public warps

A public warp appears in `/claim warps` and the public warp screen for **everyone on the server**.
Anyone with `USE_WARPS` can teleport to it.

The one safety net: teleporting to a public warp in a claim with `PVP` on shows a confirmation first,
using `warningWarpPvpEnabled`. Nobody gets dropped into a PvP zone by clicking a list entry.

## Costs and limits

| | Node | Default |
|---|---|---|
| Warps per claim | `uxmclaims.limit.warp.<n>` | `3`, MAX |
| Creating one | `uxmclaims.cost.warp.<count>.<price>` | `0.0` |
| Teleporting | `uxmclaims.cost.warptp.<public\|private>.<price>` | `0.0` |
| Warmup | `uxmclaims.delay.teleport.<n>` | `3` seconds, MIN |

The teleport cost distinguishes `public` from `private`, so a player's own warps can stay free while
hopping to someone else's shop costs money.

## Name resolution

`/claim spawn <name>` and `/claim warp teleport <name>` search public warps first, then your own
claims, then claims you are a member of, then claims by name. First match wins — a public warp named
`shop` shadows your private one.
