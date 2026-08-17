---
title: Domain model
order: 1005
description: Claim and the objects hanging off it, and the one method worth knowing.
icon: box
---

Everything lives under `com.uxplima.claim.domain.model`.

## Claim

The aggregate root.

```java
UUID    id
String  name
Instant expireDate
Instant creationDate
Location spawnLocation
ClaimBlock block
ClaimVault vault

Map<UUID, ClaimChunk>  chunks
Map<UUID, ClaimMember> members
Map<UUID, ClaimRole>   roles
Map<UUID, ClaimWarp>   warps
Map<UUID, ClaimBan>    bans
Map<UUID, ClaimInvite> invites

Set<ClaimFlag> flags
```

| Method | Returns |
|---|---|
| `getOwnerUid()` | The owner's UUID |
| `getOwner()` | The owner as a `ClaimMember` |
| `getMainChunk()` | The chunk holding the block, hologram and spawn |
| `getRemainTime()` | A `Duration` until expiry |
| `isOwner(UUID)` | Whether that player owns it |
| `hasMemberByUid(UUID)` | Whether they are a member |
| `hasBanByUid(UUID)` | Whether they are banned |
| `hasFlag(ClaimFlag)` | Whether the flag is set |
| `hasPermission(UUID, ClaimPermission)` | **The one that matters** |
| `getMemberByUid`, `getRoleById`, `getRoleByType`, `getRoleByPriority`, `getWarpByName`, `getChunkByLocation`, `getBanByUid`, `getInviteByUid` | Lookups |

### hasPermission

```java
claim.hasPermission(player.getUniqueId(), ClaimPermission.BLOCK_BREAK)
```

One call, resolving the whole chain: ban, ownership, per-member deny, per-member allow, the member's
role, the `Member` fallback when their role was deleted, and the `Default` role for non-members. Do
not reimplement it — the order is subtle and the fallbacks are easy to get wrong.

It does **not** consider `uxmclaims.admin`. For that, go through `ClaimPermissionPolicy`, which checks
the admin node first:

```java
ClaimPermissionPolicy policy = api.getInstance(ClaimPermissionPolicy.class);
policy.hasPerm(uuid, claim, ClaimPermission.BLOCK_BREAK);
policy.isOwner(uuid, claim);
policy.canPerform(uuid, ClaimAction.CLAIM_DELETE);
```

## The pieces

| Type | Holds |
|---|---|
| `ClaimMember` | `uid`, `roleId`, `joinDate`, `allowedPermissions`, `deniedPermissions` |
| `ClaimRole` | `name`, `priority`, `type`, its permission set |
| `ClaimChunk` | The chunk coordinates and world |
| `ClaimWarp` | `name`, `location`, `isPublic`, `createdBy`, `createdAt` |
| `ClaimBan` | `bannedUid`, `reason`, `bannedAt` |
| `ClaimInvite` | `invitedUid`, `invitedAt` |
| `ClaimBlock` | The style key and where the block sits |
| `ClaimVault` | The stored items |

`ClaimMember` carrying both an allowed and a denied set is the per-member override mechanism. Denied
is checked first, so a denial always wins.

## The enums

| Enum | Values | Page |
|---|---|---|
| `ClaimFlag` | 32 | [Flags](../protection/flags.md) |
| `ClaimPermission` | 48 | [Role permissions](../protection/permissions.md) |
| `ClaimAction` | 30 | [Ability permissions](../permissions/abilities.md) |
| `ClaimRoleType` | `OWNER`, `MEMBER`, `DEFAULT`, `CUSTOM` | — |
| `ClaimSideEffect` | `REGION`, `ECONOMY`, `WEBHOOK`, `LIMITATION`, `PERMISSION` | [Architecture](architecture.md) |

`ClaimAction` also builds its own permission strings — `toPermissionNode()`, `toBypassNode()`,
`toCategoryWildcard()` — which is how the ability and bypass node families stay in sync.

<Callout type="info" title="Model objects are snapshots">

A `Claim` you hold is a view of the state when you fetched it. Another server, or another thread, may
have changed it since. Re-fetch before acting on stale data, and make changes through the
[facade](api.md) rather than by mutating the object — direct mutation is not persisted.

</Callout>
