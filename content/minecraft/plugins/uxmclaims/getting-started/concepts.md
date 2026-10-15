---
title: Concepts
order: 103
description: Chunks, the claim lifecycle, roles, flags, entitlements and abilities.
icon: lightbulb
---

Everything in uxmClaims is one of six ideas. This page explains each in a few lines and links to the
page that lists the details.

## Chunks

A claim is made of whole chunks, from the bottom of the world to the top. There are no partial claims
and no selection wands.

| Rule | Detail |
|---|---|
| Main chunk | The chunk the claim was created in. It cannot be removed. |
| Adding a chunk | The chunk must be free and share a side with the claim. Diagonal contact does not count. |
| Removing a chunk | Refused when the claim would split in two: *"Cannot delete this chunk! It connects other chunks to the claim."* Warps inside the chunk are deleted, a spawn inside it moves to the main chunk, and nothing is refunded. |
| Distance | `claimSettings.minDistance` (default `2`) is the gap, in chunks, between your claim and another player's. It is checked in a square around every chunk you create or add. `0` lets claims touch. |
| Size | `uxmclaims.limit.chunk` caps the chunks in one claim, main chunk included (default 10). |

## The claim lifecycle

| Stage | What happens |
|---|---|
| Create | `/claim` or `/claim create [name]`. The chunk must be free, outside `minDistance` of other players' claims and off any WorldGuard region. The creator pays `uxmclaims.cost.claim` and must be under `uxmclaims.limit.claim`. |
| Claim block | Set where the creator stood, using `claimSettings.defaultBlock`, with a hologram above it. Nothing can break, blow up or push it. Right-clicking it opens the claim menu for members. `/claim block` moves, restyles or removes it. |
| Starting state | Flags from `claimSettings.defaultFlags`, roles from [`roles.yml`](../config/roles-yml.md), and an expiry date `claimSettings.expireTime` seconds away (30 days). |
| Time | `/claim extend <duration>` or the *Extend Time* menu adds time. It costs `uxmclaims.cost.time` per second per chunk, and the time left afterwards may not exceed `uxmclaims.limit.time` (30 days by default). |
| Expiry | With `claimSettings.expireMode: true`, a claim whose time runs out is deleted within seconds and `broadcastClaimExpired` is announced. Owners get a reminder in the last `generalSettings.reminderThresholdSeconds` (one day). |
| Delete, transfer | Only the owner may delete the claim or hand it to a member. |

## Roles

A role is a named set of [role permissions](../protection/permissions.md). Three always exist and cannot
be deleted:

| Role | Priority | Holds | Applies to |
|---|---|---|---|
| `Owner` | 0 | Every permission, implicitly | The owner |
| `Member` | 1 | The list in `roles.yml` | Everyone who accepts an invite |
| `Default` | 2 | `MOVE_INSIDE` | Everyone who is not a member: visitors |

A lower priority number is a higher rank. A custom role is inserted just above `Default`, so it ranks
below `Member` until you change its priority, and it starts with no permissions at all. Give it
`MOVE_INSIDE` first: without it, its holders are pushed out of the claim. Deleting a custom role moves
its members to `Member`.

## Flags

A [flag](../protection/flags.md) is a rule about the claim itself, not about a player. `TNT_EXPLOSIONS`
does not ask who lit the TNT; it decides whether TNT may break blocks here at all.

Flags are **allow when present**. A flag in the claim's set permits that thing; removing it forbids it.
Only the owner changes flags.

## The permission check

When a player acts inside a claim, uxmClaims asks in a fixed order: admin, ban, owner, the member's own
overrides, the member's role, and for everyone else the `Default` role. The full table is on
[How protection works](../protection/overview.md). Two consequences matter in practice:

- A ban beats every role and every override.
- A per-member deny beats the role. You can give someone the Builder role and still take
  `CONTAINER_OPEN` away from them alone.

## Entitlements

An entitlement is a number that comes from a permission node: how many claims a player may own, how
many chunks a claim may have, what a chunk costs, how long a teleport warmup lasts. Each is defined in
[`entitlements.yml`](../config/entitlements-yml.md) with a default and a strategy for limits and delays:
`STACK` adds every grant to the default, `MAX` takes the largest, `MIN` the smallest. For a cost, the
cheapest matching grant wins.

uxmClaims reads these nodes through LuckPerms. Without LuckPerms, every player gets the defaults.

## Abilities

An ability node, `uxmclaims.ability.<category>.<action>`, decides whether a player may perform an
operation anywhere at all: create claims, delete them, make warps, open vaults. A role permission then
decides whether they may do it in this claim. Both must pass. Without LuckPerms every ability passes;
with LuckPerms nothing passes until you grant it. See [Ability permissions](../permissions/abilities.md).
