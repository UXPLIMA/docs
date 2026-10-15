---
title: Warps
order: 207
description: Creating warps inside a claim, moving them, and publishing them server-wide.
icon: map-pin
---

A warp is a named teleport point inside a claim. Members reach the warps of their claims, and a warp
made public is open to every player on the server.

## Commands

| Command | What it does | Claim access | Ability node |
|---|---|---|---|
| `/claim warp create <name>` | Creates a private warp where you stand | `MANAGE_WARPS` | `uxmclaims.ability.warp.create` |
| `/claim warp delete <warp>` | Deletes a warp, after confirmation | `MANAGE_WARPS` | `uxmclaims.ability.warp.delete` |
| `/claim warp rename <warp> <name>` | Renames a warp | `MANAGE_WARPS` | `uxmclaims.ability.warp.rename` |
| `/claim warp move <warp>` | Moves a warp to where you stand | `MANAGE_WARPS` | `uxmclaims.ability.warp.relocate` |
| `/claim warp setpublic <warp> <true\|false>` | Publishes or hides a warp | `MANAGE_WARPS` | `uxmclaims.ability.warp.visibility` |
| `/claim warp teleport <warp>` | Teleports you to a warp you can reach | `USE_WARPS` for a private warp | none |

All but `teleport` act on the warps of the current claim. `create` and `move` only accept a position
inside the claim's chunks (`errorLocationOutsideChunk`). Warp names are unique within a claim
regardless of case. In `rename`, the old name is one word and the new name takes the rest of the line.

`/claim warps` opens the list of public warps, and `/claim spawn <name>` also finds warps: see
[Claim commands](claim.md#spawn-warps-and-the-vault).

### Which warps you can reach

`/claim warp teleport <warp>` and `/claim spawn <name>` look in this order and take the first match:

1. public warps of every claim
2. warps in claims you own
3. warps in claims you belong to
4. for `/claim spawn` only: claims you own or belong to, by name, to their spawn

A public warp named `shop` shadows your own warp named `shop`. Name public warps distinctly if that
matters on your server.

| Warp | Who may teleport |
|---|---|
| Public | Everyone |
| Private | The owner, holders of `uxmclaims.admin`, and members whose role or per-member override gives `USE_WARPS` in that claim |

A refused private warp answers with `errorNoPermission`. The Teleport button in the warp manage menu
applies the same rule.

Teleporting by command to a public warp in a claim with the `PVP` flag on asks for confirmation
first, with `warningWarpPvpEnabled`. A click in the warp list menus teleports without this warning.

A teleport waits out the warmup and is cancelled if you move. The price is taken after the warmup. A
player banned from the claim, or whose role there lacks `MOVE_INSIDE`, cannot teleport into it.

## Permissions

| Node | Applies to | Default |
|---|---|---|
| `uxmclaims.limit.warp.<n>` | Warps per claim, read from the owner | `3` |
| `uxmclaims.cost.warp.<count>.<price>` | Price of creating a warp | `0.0` |
| `uxmclaims.cost.warptp.<public\|private>.<price>` | Price of a warp teleport | `0.0` |
| `uxmclaims.delay.warprename.<n>` | Seconds between warp renames | `30` |
| `uxmclaims.delay.warprelocate.<n>` | Seconds between warp moves | `60` |
| `uxmclaims.delay.teleport.<n>` | Teleport warmup in seconds | `3` |

The teleport price separates public from private warps, so a server can keep a player's own warps free
and charge for visiting someone else's. Delays use the `MIN` strategy: granting a rank
`uxmclaims.delay.teleport.0` makes its teleports instant. See
[Limits, costs and delays](../permissions/limits.md).

## Notes

- The default `Member` role has `USE_WARPS`. A custom role created in game starts empty: give it
  `USE_WARPS` if its members should use private warps.
- Removing a chunk deletes the warps inside it: see [Chunks](chunks.md).
- The [warp menus](../menus/warps.md) create, edit and publish warps with the same checks.

Related: [Claim commands](claim.md), [Warp menus](../menus/warps.md), [Limits, costs and delays](../permissions/limits.md)
