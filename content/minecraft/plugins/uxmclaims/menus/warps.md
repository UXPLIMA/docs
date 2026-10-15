---
title: Warps
order: 409
description: Creating warps in a claim, and publishing them to the server.
icon: map-pin
---

Two screens, `claim_warp_list.yml` and `claim_warp_manage.yml`, opened by **Warps** in claim
management. Names are typed through `claim_warp_create.yml` and `claim_warp_rename.yml`, which use a
dialog and fall back to an anvil (see [typing text](overview.md#typing-text)).

Every warp action needs the `MANAGE_WARPS` role permission and its ability node.

## The list

Every warp in the claim, showing its location, whether it is public, and when it was created. Clicking
opens the management screen for that warp.

**Create** (`addWarp`) asks for a name and places the warp where you stand. In the dialog, the
**Visible to everyone** switch makes it public from the start.

## Managing one

| Button | Key | Does | Ability node |
|---|---|---|---|
| Teleport | `teleport` | Goes there; a private warp needs `USE_WARPS` | n/a |
| Move here | `relocate` | Moves it to where you stand | `uxmclaims.ability.warp.relocate` |
| Rename | `rename` | Asks for a new name | `uxmclaims.ability.warp.rename` |
| Public / private | `visibility` | Toggles visibility | `uxmclaims.ability.warp.visibility` |
| Delete | `delete` | Removes it, after confirmation | `uxmclaims.ability.warp.delete` |

Creating needs `uxmclaims.ability.warp.create`.

## Public warps

A public warp appears in `/claim warps` and the [public warp screen](personal.md#public-warps) for
**everyone on the server**, and anyone may teleport to it except players banned from that claim.

The one safety net: teleporting to a public warp in a claim with `PVP` on asks first. The menu shows
the confirmation screen, and the commands send `warningWarpPvpEnabled`. Nobody gets dropped into a PvP
zone by clicking a list entry.

## Costs and limits

| | Node | Default |
|---|---|---|
| Warps per claim | `uxmclaims.limit.warp.<n>` | `3`, MAX |
| Creating one | `uxmclaims.cost.warp.<n>.<price>` | `0.0` |
| Teleporting | `uxmclaims.cost.warptp.<public\|private>.<price>` | `0.0` |
| Warmup | `uxmclaims.delay.teleport.<n>` | `3` seconds, MIN |

The teleport cost distinguishes `public` from `private`, so a player's own warps can stay free while
hopping to someone else's shop costs money.

## Name resolution

`/claim warp teleport <name>` searches public warps first, then warps in claims you own, then claims
you are a member of. First match wins: a public warp named `shop` shadows your private one.
`/claim spawn <name>` searches the same way, then falls back to a claim with that name.
