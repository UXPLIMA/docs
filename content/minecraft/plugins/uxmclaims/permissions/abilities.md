---
title: Ability permissions
order: 602
description: The uxmclaims.ability.* nodes, which command each gates, and the wildcards.
icon: key-round
---

An ability node decides whether a player may perform an operation **anywhere at all**. It is separate
from the [role permission](../protection/permissions.md), which decides whether they may do it *in
this claim*. Both must pass. The menu buttons that perform the same operation check the same node.

With LuckPerms and `permissionSupport: true`, a node you do not grant is denied, unless the player is
an admin or holds a matching bypass. Without them, every ability is granted: see
[What reads the nodes](reference.md#what-reads-the-nodes).

## Claim lifecycle

| Node | Gates |
|---|---|
| `uxmclaims.ability.claim.create` | `/claim create`, and `/claim` when it creates a claim |
| `uxmclaims.ability.claim.delete` | `/claim delete` |
| `uxmclaims.ability.claim.rename` | `/claim rename` |
| `uxmclaims.ability.claim.relocate` | `/claim setspawn` |
| `uxmclaims.ability.claim.reschedule` | `/claim extend`, the time menu |
| `uxmclaims.ability.claim.transfer` | `/claim transfer`, `/claim member transfer` |
| `uxmclaims.ability.claim.flag.<flag>` | `/claim setflag`, `/claim toggleflag` and the flag menu, for that flag |
| `uxmclaims.ability.claim.flag.*` | All flags |

`<flag>` is the [flag name](../protection/flags.md) in lower case: `uxmclaims.ability.claim.flag.pvp`,
`uxmclaims.ability.claim.flag.keep_inventory`.

## Chunks

| Node | Gates |
|---|---|
| `uxmclaims.ability.chunk.extend` | `/claim chunk add`, and `/claim` when it extends a claim |
| `uxmclaims.ability.chunk.shrink` | `/claim chunk remove` |

With `smartCommand: true` in `config.yml`, `/claim` outside a claim extends your own claim when one
borders the chunk, and creates a new claim otherwise. Each path checks its own node.

## Members

| Node | Gates |
|---|---|
| `uxmclaims.ability.member.invite` | `/claim trust`, `/claim invite send` |
| `uxmclaims.ability.member.revoke` | `/claim invite revoke` |
| `uxmclaims.ability.member.kick` | `/claim kick`, `/claim member kick` |
| `uxmclaims.ability.member.ban` | `/claim ban` |
| `uxmclaims.ability.member.unban` | `/claim unban` |
| `uxmclaims.ability.member.role` | `/claim member setrole`, `promote`, `demote` |
| `uxmclaims.ability.member.permissions.<perm>` | `/claim member setperm` for that permission |
| `uxmclaims.ability.member.permissions.*` | All of them |

`<perm>` is the [role permission](../protection/permissions.md) name in lower case:
`uxmclaims.ability.member.permissions.block_break`. Declining an invitation sent to you
(`/claim invite reject`) needs no node.

## Roles

| Node | Gates |
|---|---|
| `uxmclaims.ability.role.create` | `/claim role create` |
| `uxmclaims.ability.role.delete` | `/claim role delete` |
| `uxmclaims.ability.role.rename` | `/claim role rename` |
| `uxmclaims.ability.role.priority` | `/claim role setpriority` |
| `uxmclaims.ability.role.permissions.<perm>` | `/claim role setperm` for that permission |
| `uxmclaims.ability.role.permissions.*` | All of them |

## Warps

| Node | Gates |
|---|---|
| `uxmclaims.ability.warp.create` | `/claim warp create` |
| `uxmclaims.ability.warp.delete` | `/claim warp delete` |
| `uxmclaims.ability.warp.rename` | `/claim warp rename` |
| `uxmclaims.ability.warp.relocate` | `/claim warp move` |
| `uxmclaims.ability.warp.visibility` | `/claim warp setpublic` |

Teleporting to a warp needs no ability node: every public warp, and every warp of a claim you own or
belong to, is open to you.

## Claim block and vault

| Node | Gates |
|---|---|
| `uxmclaims.ability.block.place` | `/claim block place` |
| `uxmclaims.ability.block.change` | `/claim block change` |
| `uxmclaims.ability.block.destroy` | `/claim block destroy` |
| `uxmclaims.ability.vault.access` | `/claim vault`, opening and changing the vault |

## Wildcards

| Node | Grants |
|---|---|
| `uxmclaims.ability.*` | Every ability |
| `uxmclaims.ability.<category>.*` | Every ability in that category |

The categories are `claim`, `chunk`, `member`, `role`, `warp`, `block` and `vault`.

## The order they are checked

For every action, `ClaimPermissionPolicy.canPerform` asks in this order and stops at the first yes:

1. Is the player an admin: an operator, or holding `uxmclaims.admin`?
2. `uxmclaims.bypass.*`, then the bypass category wildcard, then the exact bypass node
3. `uxmclaims.ability.*`, then the ability category wildcard
4. The wildcard the action declares: `…claim.flag.*`, `…member.permissions.*` or `…role.permissions.*`
5. The exact ability node

<Callout type="tip" title="Grant per-flag nodes to shape what players may change">

`uxmclaims.ability.claim.flag.*` hands a player every flag, `pvp` included. Granting only
`uxmclaims.ability.claim.flag.keep_inventory` and `uxmclaims.ability.claim.flag.mob_spawning` lets
players tune what affects only them, while PvP stays a server decision.

</Callout>
