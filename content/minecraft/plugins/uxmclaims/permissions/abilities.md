---
title: Ability permissions
order: 602
description: The uxmclaims.ability.* nodes, which command each gates, and the wildcards.
icon: key-round
---

An ability node decides whether a player may perform an operation **anywhere at all**. It is separate
from the [role permission](../protection/permissions.md), which decides whether they may do it *in
this claim*. Both must pass.

There is no default. A node not granted is denied, except for a player holding `uxmclaims.admin` or a
matching bypass.

## Claim lifecycle

| Node | Gates |
|---|---|
| `uxmclaims.ability.claim.create` | `/claim create`, and the smart `/claim` |
| `uxmclaims.ability.claim.delete` | `/claim delete` |
| `uxmclaims.ability.claim.rename` | `/claim rename` |
| `uxmclaims.ability.claim.relocate` | `/claim setspawn` |
| `uxmclaims.ability.claim.reschedule` | `/claim extend` |
| `uxmclaims.ability.claim.transfer` | `/claim transfer`, `/claim member transfer` |
| `uxmclaims.ability.claim.flag.<FLAG>` | `/claim setflag`, `/claim toggleflag` for that flag |
| `uxmclaims.ability.claim.flag.*` | All flags |

## Chunks

| Node | Gates |
|---|---|
| `uxmclaims.ability.chunk.extend` | `/claim chunk add` |
| `uxmclaims.ability.chunk.shrink` | `/claim chunk remove` |

## Members

| Node | Gates |
|---|---|
| `uxmclaims.ability.member.invite` | `/claim trust`, `/claim invite send` |
| `uxmclaims.ability.member.revoke` | `/claim invite revoke` |
| `uxmclaims.ability.member.kick` | `/claim kick`, `/claim member kick` |
| `uxmclaims.ability.member.ban` | `/claim ban` |
| `uxmclaims.ability.member.unban` | `/claim unban` |
| `uxmclaims.ability.member.role` | `/claim member setrole`, `promote`, `demote` |
| `uxmclaims.ability.member.permissions.<PERM>` | `/claim member setperm` for that permission |
| `uxmclaims.ability.member.permissions.*` | All of them |

## Roles

| Node | Gates |
|---|---|
| `uxmclaims.ability.role.create` | `/claim role create` |
| `uxmclaims.ability.role.delete` | `/claim role delete` |
| `uxmclaims.ability.role.rename` | `/claim role rename` |
| `uxmclaims.ability.role.priority` | `/claim role setpriority` |
| `uxmclaims.ability.role.permissions.<PERM>` | `/claim role setperm` for that permission |
| `uxmclaims.ability.role.permissions.*` | All of them |

## Warps

| Node | Gates |
|---|---|
| `uxmclaims.ability.warp.create` | `/claim warp create` |
| `uxmclaims.ability.warp.delete` | `/claim warp delete` |
| `uxmclaims.ability.warp.rename` | `/claim warp rename` |
| `uxmclaims.ability.warp.relocate` | `/claim warp move` |
| `uxmclaims.ability.warp.visibility` | `/claim warp setpublic` |

## Claim block and vault

| Node | Gates |
|---|---|
| `uxmclaims.ability.block.place` | `/claim block place` |
| `uxmclaims.ability.block.change` | `/claim block change` |
| `uxmclaims.ability.block.destroy` | `/claim block destroy` |
| `uxmclaims.ability.vault.access` | `/claim vault` |

## Wildcards

| Node | Grants |
|---|---|
| `uxmclaims.ability.*` | Every ability |
| `uxmclaims.ability.<category>.*` | Every ability in that category |

The categories are `claim`, `chunk`, `member`, `role`, `warp`, `block` and `vault`: the part of the
enum name before the underscore.

## The order they are checked

For any action, `ClaimPermissionPolicy` asks in this order and stops at the first yes:

1. Is the player `uxmclaims.admin`?
2. `uxmclaims.bypass.*`, then the bypass category wildcard, then the exact bypass node
3. `uxmclaims.ability.*`, then the ability category wildcard
4. Any explicit wildcards the action declares, `…flag.*`, `…permissions.*`
5. The exact ability node

<Callout type="tip" title="Grant per-flag nodes to shape what players may change">

`uxmclaims.ability.claim.flag.*` hands a player every flag, `PVP` included. Granting only
`uxmclaims.ability.claim.flag.KEEP_INVENTORY` and `…flag.MOB_SPAWNING` lets players tune the things
that affect only them, while PvP stays a server decision.

</Callout>
