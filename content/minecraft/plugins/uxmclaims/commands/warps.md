---
title: Warps
order: 207
description: Creating warps inside a claim, moving them, and publishing them server-wide.
icon: map-pin
---

| Command | What it does | Ability node |
|---|---|---|
| `/claim warp create <name>` | Create a warp where you stand | `uxmclaims.ability.warp.create` |
| `/claim warp delete <name>` | Delete it | `uxmclaims.ability.warp.delete` |
| `/claim warp rename <name> <new>` | Rename it | `uxmclaims.ability.warp.rename` |
| `/claim warp move <name>` | Move it to where you stand | `uxmclaims.ability.warp.relocate` |
| `/claim warp setpublic <name> <true\|false>` | Publish or unpublish it | `uxmclaims.ability.warp.visibility` |
| `/claim warp teleport <name>` | Teleport to any warp you may reach | — |

Managing warps also needs the `MANAGE_WARPS` role permission. *Using* one needs `USE_WARPS`.

`/claim warps` opens the public warp list, and `/claim spawn <name>` will find a warp by name too.

## Public and private

A private warp is visible to the claim's members. A public warp appears in `/claim warps` for
everyone on the server and can be teleported to by anyone with `USE_WARPS`.

Publishing is the one thing here with a safety net: teleporting to a public warp in a claim that has
the `PVP` flag on prompts for confirmation first, with the `warningWarpPvpEnabled` message. A player
cannot be dropped into a PvP zone by clicking a warp in a list.

## What a teleport costs

| Setting | Node | Default |
|---|---|---|
| Warps per claim | `uxmclaims.limit.warp.<n>` | `3`, MAX |
| Creating a warp | `uxmclaims.cost.warp.<count>.<price>` | `0.0` |
| Teleporting | `uxmclaims.cost.warptp.<public\|private>.<price>` | `0.0` |
| Warmup before the teleport | `uxmclaims.delay.teleport.<n>` | `3` seconds, MIN |

The teleport cost node distinguishes public from private, so you can leave a player's own warps free
and charge for hopping to someone else's shop.

The delay uses the `MIN` strategy, which means granting a rank a *smaller* number is the perk:
`uxmclaims.delay.teleport.0` is an instant teleport.

## Name resolution

`/claim spawn <name>` and `/claim warp teleport <name>` search in this order:

1. public warps
2. warps in claims you own
3. warps in claims you are a member of
4. claims by name, teleporting to the claim spawn

First match wins. A public warp named `shop` will shadow your own private warp of the same name, so
name public warps distinctly if that matters on your server.
