---
title: The API
order: 1003
description: The entry point, the two facades, and every operation they expose.
icon: terminal
---

## Entry point

```java
UxmClaimBukkitAPI api = UxmClaimBukkitAPI.getInstance();

ClaimFacade claims   = api.claimFacade();
PlayerFacade players = api.playerFacade();
```

`getInstance()` throws `IllegalStateException` before uxmClaims has initialised. Call it from
`onEnable` with `softdepend: [uxmClaims]` set.

The API can also resolve anything from the container:

```java
SomeService service = api.getInstance(SomeService.class);
SomeService named   = api.getInstance(SomeService.class, "key");
```

## Reading

Queries are plain methods. Nothing here mutates.

| Method | Returns |
|---|---|
| `findById(UUID)` | `Optional<Claim>` |
| `findByChunk(Chunk)` | `Optional<Claim>` |
| `findByLocation(Location)` | `Optional<Claim>` |
| `getById(UUID)` / `getByChunk` / `getByLocation` | `Claim`, throwing when absent |
| `getByIdUnsafe` / `getByChunkUnsafe` / `getByLocationUnsafe` | `Claim` or `null` |
| `existsById` / `existsByChunk` / `existsByLocation` / `existsByWarpName` | `boolean` |
| `findByOwnerUid(UUID)` | `List<Claim>` |
| `findByMemberUid(UUID)` | `List<Claim>` |
| `findByInvitedUid(UUID)` | `List<Claim>` |
| `countByOwnerUid(UUID)` / `countByMemberUid(UUID)` | `int` |
| `findAllClaims()` | `List<Claim>` |
| `findExpiredClaims()` | `List<Claim>` |
| `findPublicWarps()` | `List<ClaimWarp>` |

The three families differ in how they handle absence: `find…` gives you an `Optional`, `get…` throws,
`get…Unsafe` returns `null`. Prefer `find…` unless you have already checked.

```java
boolean canBuild = claims.findByLocation(loc)
        .map(claim -> claim.hasPermission(player.getUniqueId(), ClaimPermission.BLOCK_PLACE))
        .orElse(true);   // wilderness is not protected
```

That is the whole of "may this player build here", including bans, ownership, per-member overrides
and role fallback, because `hasPermission` resolves all of it.

## Writing

Every write takes a command object and has an overload taking `ClaimCommandOptions`.

```java
Claim claim = claims.renameClaim(
        ClaimRenameCommand.builder()
                .claimId(id)
                .actorUid(actor)
                .name("New name")
                .build());
```

| Area | Methods |
|---|---|
| Claim | `createClaim`, `deleteClaim`, `expireClaim`, `renameClaim`, `relocateClaim`, `rescheduleClaim`, `updateClaimFlags` |
| Members | `inviteMember`, `revokeInvite`, `acceptInvite`, `kickMember`, `leaveMember`, `banMember`, `unbanMember`, `updateMemberPerms`, `assignMemberRole`, `transferOwnership` |
| Chunks | `extendClaim`, `shrinkClaim` |
| Claim block | `placeBlock`, `changeBlock`, `destroyBlock` |
| Vault | `updateVault` |
| Roles | `createRole`, `deleteRole`, `renameRole`, `updateRolePriority`, `updateRolePermission` |
| Warps | `createWarp`, `deleteWarp`, `renameWarp`, `relocateWarp`, `updateWarpVisibility` |

Each command class mirrors its method name (`ClaimRenameCommand`, `ClaimMemberBanCommand`,
`ClaimWarpCreateCommand`) and all of them carry the `actorUid` the permission check runs against.

## Controlling side effects

```java
claims.createClaim(command, ClaimCommandOptions.builder()
        .without(ClaimSideEffect.ECONOMY)
        .without(ClaimSideEffect.WEBHOOK)
        .build());
```

| Effect | Skipping it means |
|---|---|
| `ECONOMY` | Nothing is charged |
| `WEBHOOK` | Discord is not notified |
| `REGION` | WorldGuard overlap is not checked |
| `LIMITATION` | Entitlement limits are not enforced |
| `PERMISSION` | Ability and role permissions are not enforced |

`ClaimCommandOptions.all()` is the default; `none()` disables everything, including the two checks you
usually want. Prefer removing effects individually.

## Threading

Writes touch the database. Call them off the main thread, or accept the tick cost: the facade does
not schedule for you. Reads are served from cache and are cheap.

## PlayerFacade

`api.playerFacade()` resolves the plugin's `CPlayer` records by UUID or name, which is what turns a
name typed in chat into something the command objects accept.
