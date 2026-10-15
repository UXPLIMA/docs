---
title: Domain model
order: 1005
description: Claim and the objects hanging off it, and the one method worth knowing.
icon: box
---

The model lives in `com.uxplima.claim.domain.model`, with the enums in `.enums` and the coordinate
types in `.vo`. Read it freely; change it only through the [facade](api.md).

## Claim

The aggregate root. Every other object on this page belongs to one claim.

| Getter | Type |
|---|---|
| `getId()` | `UUID` |
| `getName()` | `String` |
| `getCreationDate()` / `getExpireDate()` | `Instant` |
| `getSpawnLocation()` | domain `Location` |
| `getBlock()` | `ClaimBlock` |
| `getVault()` | `ClaimVault` |
| `getChunks()` / `getMembers()` / `getRoles()` / `getWarps()` / `getBans()` / `getInvites()` | Read-only `Map<UUID, …>` |
| `getFlags()` | Read-only `Set<ClaimFlag>` |

The maps are keyed by each entry's own id, not by a player's UUID. To find a player, use the `…ByUid`
lookups below.

| Method | Returns |
|---|---|
| `hasPermission(UUID, ClaimPermission)` | Whether that player may do it here. **The one that matters** |
| `isOwner(UUID)` | Whether that player owns it |
| `getOwnerUid()` / `getOwner()` / `findOwner()` | The owner's UUID, as `ClaimMember`, or as `Optional` |
| `getMainChunk()` / `findMainChunk()` | The chunk holding the block, hologram and spawn |
| `getRemainTime()` / `isExpired()` | A `Duration` until expiry, negative once expired |
| `hasFlag(ClaimFlag)` | Whether the flag is set |
| `has…` / `find…` / `get…` + `MemberByUid`, `BanByUid`, `InviteByUid` | A player's membership, ban or invite |
| `has…` / `find…` / `get…` + `RoleById`, `RoleByType`, `RoleByPriority` | A role |
| `has…` / `find…` / `get…` + `WarpById`, `WarpByName` | A warp |
| `has…` / `find…` / `get…` + `ChunkById`, `ChunkByData(Chunk)`, `ChunkByLocation(Location)` | A chunk |
| `has…` / `find…` / `get…` + `MemberById`, `BanById`, `InviteById` | An entry by its own id |

`has…` returns `boolean`, `find…` an `Optional`, and `get…` the object or a `ClaimException`.

### hasPermission

```java
claim.hasPermission(player.getUniqueId(), ClaimPermission.BLOCK_BREAK)
```

One call resolves the whole chain, in this order:

| Step | Result |
|---|---|
| The player is banned | Denied |
| The player is the owner | Allowed |
| The member has a per-member deny | Denied |
| The member has a per-member allow | Allowed |
| The member's role, or the `MEMBER` role if theirs was deleted | That role's answer |
| Not a member | The `DEFAULT` role's answer |

A missing role denies rather than throws, and a `null` argument returns `false`. Do not reimplement
the chain.

It does not consider `uxmclaims.admin`. `ClaimPermissionPolicy` does: it treats operators and holders
of `uxmclaims.admin` as allowed, then falls through to the claim.

```java
ClaimPermissionPolicy policy = api.getInstance(ClaimPermissionPolicy.class);

policy.hasPerm(uuid, claim, ClaimPermission.BLOCK_BREAK);
policy.isOwner(uuid, claim);
policy.canPerform(uuid, ClaimAction.CLAIM_DELETE);
```

`canPerform` checks an [ability node](../permissions/abilities.md), with bypass and wildcard nodes
first.

## The pieces

| Type | Getters |
|---|---|
| `ClaimMember` | `getId`, `getUid`, `getClaimId`, `getRoleId`, `getJoinDate`, `getAllowedPermissions`, `getDeniedPermissions`, `hasAllowedPermission`, `hasDeniedPermission` |
| `ClaimRole` | `getId`, `getClaimId`, `getName`, `getPriority`, `getType`, `getPermissions`, `hasPermission`, `isOwner`, `isMember`, `isDefault`, `isCustom` |
| `ClaimChunk` | `getId`, `getClaimId`, `getChunk`, `getCreatedAt`, `isMain` |
| `ClaimWarp` | `getId`, `getClaimId`, `getName`, `getLocation`, `isPublic`, `getCreatedBy`, `getCreatedAt` |
| `ClaimBan` | `getId`, `getClaimId`, `getActorUid`, `getBannedUid`, `getReason`, `getBannedAt` |
| `ClaimInvite` | `getId`, `getClaimId`, `getActorUid`, `getInvitedUid`, `getInvitedAt` |
| `ClaimBlock` | `getId`, `getClaimId`, `getKeyId`, `getLocation`, `isPlaced` |
| `ClaimVault` | `getId`, `getClaimId`, `getActorUid`, `getUpdatedAt`, `getItems` (`List<ClaimVaultItem>`) |
| `ClaimVaultItem` | `getVaultId`, `getSlot`, `getItemStack` (the stack, serialised to a `String`) |
| `CPlayer` | `getUid`, `getName`, `getTexture`, `getSignature`, `getFirstLoginDate`, `getLastLoginDate` |
| `vo.Location` | `getWorld` (name), `getX`, `getY`, `getZ`, `getChunk`, `getBlockLocation` |
| `vo.Chunk` | `getWorld` (name), `getX`, `getZ`, `getCenter`, `isNeighbor` |

A member's denied set is checked before the allowed set, so a denial always wins. Convert `Location`
and `Chunk` to Bukkit with [`BukkitConverter`](api.md).

## The enums

| Enum | Values | Page |
|---|---|---|
| `ClaimFlag` | 32 | [Flags](../protection/flags.md) |
| `ClaimPermission` | 48 | [Role permissions](../protection/permissions.md) |
| `ClaimAction` | 30 | [Ability permissions](../permissions/abilities.md) |
| `ClaimRoleType` | `OWNER`, `MEMBER`, `DEFAULT`, `CUSTOM` | [Roles](../commands/roles.md) |

`ClaimAction` builds its own permission strings (`toPermissionNode(…)`, `toBypassNode(…)`,
`toCategoryWildcard()`, `toBypassCategoryWildcard()`), which keeps the ability and bypass node
families in sync. `ClaimSideEffect` is not a domain enum: it lives in
`com.uxplima.claim.app.command.options`, see [The API](api.md).

<Callout type="info" title="Model objects are copies">

Every `Claim` the facade returns is a copy of the plugin's cached state at the moment you fetched
it. Mutating methods such as `rename` or `addFlag` change only your copy: nothing is saved, no event
fires, and the plugin never sees it. Make changes through the [facade](api.md), and fetch again
after a write instead of reusing an old copy.

</Callout>

Related: [The API](api.md), [Events](events.md)
