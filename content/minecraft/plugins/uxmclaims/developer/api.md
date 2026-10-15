---
title: The API
order: 1003
description: The entry point, the two facades, and every operation they expose.
icon: terminal
---

Everything starts at `UxmClaimBukkitAPI`, which hands you `ClaimFacade` for claims and
`PlayerFacade` for the plugin's player records. Reads are plain methods; writes take a command object.

## Entry point

```java
UxmClaimBukkitAPI api = UxmClaimBukkitAPI.getInstance();

ClaimFacade claims   = api.claimFacade();
PlayerFacade players = api.playerFacade();
```

| Method | Returns |
|---|---|
| `getInstance()` (static) | The API, or `IllegalStateException` before uxmClaims has started |
| `claimFacade()` | `ClaimFacade` |
| `playerFacade()` | `PlayerFacade` |
| `eventBusPort()` | `EventBusPort`, the [event bus](events.md) |
| `registerEvent(Object)` / `unregisterEvent(Object)` | Adds or removes an event listener |
| `getInstance(Class<T>)` / `getInstance(Class<T>, String)` | Any object from the plugin's container, such as `ClaimPermissionPolicy` |

## Domain coordinates

The facades take the plugin's own `Location` and `Chunk` (package `com.uxplima.claim.domain.model.vo`),
not Bukkit's. `BukkitConverter` translates in both directions.

| Method | Converts |
|---|---|
| `toDomainLocation(org.bukkit.Location)` | Bukkit location to domain `Location` |
| `toDomainChunk(org.bukkit.Chunk)` / `toDomainChunk(org.bukkit.Location)` | Bukkit chunk or location to domain `Chunk` |
| `toBukkitLocation(Location)` / `toBukkitChunk(Chunk)` | Back to Bukkit, `null` when the world is not loaded |

## Reading

| Method | Returns |
|---|---|
| `findById(UUID)` / `findByChunk(Chunk)` / `findByLocation(Location)` | `Optional<Claim>` |
| `getById` / `getByChunk` / `getByLocation` | `Claim`, or `ClaimException` (`ERROR_CLAIM_NOT_FOUND`) when absent |
| `getByIdUnsafe` / `getByChunkUnsafe` / `getByLocationUnsafe` | `Claim` or `null` |
| `existsById` / `existsByChunk` / `existsByLocation` / `existsByWarpName(String)` | `boolean` |
| `findByOwnerUid(UUID)` / `findByMemberUid(UUID)` / `findByInvitedUid(UUID)` | `List<Claim>` |
| `countByOwnerUid(UUID)` / `countByMemberUid(UUID)` | `int` |
| `findAllClaims()` / `findExpiredClaims()` | `List<Claim>` |
| `findPublicWarps()` | `List<ClaimWarp>` |

Reads come from memory, not the database, and every `Claim` returned is a copy. Prefer `find…`;
use `get…Unsafe` only after a check.

```java
Location here = BukkitConverter.toDomainLocation(player.getLocation());

boolean canBuild = claims.findByLocation(here)
        .map(claim -> claim.hasPermission(player.getUniqueId(), ClaimPermission.BLOCK_PLACE))
        .orElse(true);   // wilderness is not protected
```

`hasPermission` resolves bans, ownership, per-member overrides and the role chain. It ignores
`uxmclaims.admin`; see [Domain model](models.md) for the check that includes it.

## Writing

```java
try {
    Claim renamed = claims.renameClaim(ClaimRenameCommand.builder()
            .claimId(claim.getId())
            .actorUid(player.getUniqueId())
            .name("Harbour")
            .build());
} catch (ClaimException e) {
    getLogger().info("Rename refused: " + e.getStatus());
}
```

Every command is a record in `com.uxplima.claim.app.command` with a Lombok builder. Its constructor
validates the fields and throws `ClaimException` (`ERROR_INVALID_INPUT`) for a missing one. A refused
write throws `ClaimException` too, and `getStatus()` names the reason.

| Method | Command | Fields | Returns |
|---|---|---|---|
| `createClaim` | `ClaimCreateCommand` | `actorUid`, `ownerUid`, `claimName`, `spawnLocation` | `Claim` |
| `deleteClaim` | `ClaimDeleteCommand` | `claimId`, `actorUid` | `Claim` |
| `expireClaim` | `ClaimExpireCommand` | `claimId` | `Claim` |
| `renameClaim` | `ClaimRenameCommand` | `claimId`, `actorUid`, `name` | `Claim` |
| `relocateClaim` | `ClaimRelocateCommand` | `claimId`, `actorUid`, `location` | `Claim` |
| `rescheduleClaim` | `ClaimRescheduleCommand` | `claimId`, `actorUid`, `adjustment` (`Duration`) | `Claim` |
| `updateClaimFlags` | `ClaimFlagsUpdateCommand` | `claimId`, `actorUid`, `flags` | `Claim` |
| `inviteMember` | `ClaimInviteSendCommand` | `claimId`, `actorUid`, `invitedUid` | `ClaimInvite` |
| `revokeInvite` | `ClaimInviteRevokeCommand` | `claimId`, `actorUid`, `invitedUid` | `ClaimInvite` |
| `acceptInvite` | `ClaimInviteAcceptCommand` | `claimId`, `invitedUid` | `ClaimMember` |
| `kickMember` | `ClaimMemberKickCommand` | `claimId`, `actorUid`, `memberUid` | `ClaimMember` |
| `leaveMember` | `ClaimMemberLeaveCommand` | `claimId`, `memberUid` | `ClaimMember` |
| `banMember` | `ClaimMemberBanCommand` | `claimId`, `actorUid`, `bannedUid`, `reason` | `ClaimBan` |
| `unbanMember` | `ClaimMemberUnbanCommand` | `claimId`, `actorUid`, `bannedUid` | `ClaimBan` |
| `updateMemberPerms` | `ClaimMemberPermissionUpdateCommand` | `claimId`, `actorUid`, `memberUid`, `deniedPermissions`, `allowedPermissions` | `ClaimMember` |
| `assignMemberRole` | `ClaimMemberRoleAssignCommand` | `claimId`, `actorUid`, `memberUid`, `roleId` | `ClaimMember` |
| `transferOwnership` | `ClaimOwnerTransferCommand` | `claimId`, `actorUid`, `newOwnerUid` | `ClaimMember` |
| `extendClaim` | `ClaimChunkExtendCommand` | `claimId`, `actorUid`, `chunk` | `ClaimChunk` |
| `shrinkClaim` | `ClaimChunkShrinkCommand` | `claimId`, `actorUid`, `chunk` | `ClaimChunk` |
| `placeBlock` | `ClaimBlockPlaceCommand` | `claimId`, `actorUid`, `blockKeyId`, `location` | `ClaimBlock` |
| `changeBlock` | `ClaimBlockChangeCommand` | `claimId`, `actorUid`, `blockKeyId` | `ClaimBlock` |
| `destroyBlock` | `ClaimBlockDestroyCommand` | `claimId`, `actorUid` | `ClaimBlock` |
| `updateVault` | `ClaimVaultUpdateCommand` | `claimId`, `actorUid`, `items` (`List<Item>` of slot and serialised stack) | `ClaimVault` |
| `createRole` | `ClaimRoleCreateCommand` | `claimId`, `actorUid`, `name` | `ClaimRole` |
| `deleteRole` | `ClaimRoleDeleteCommand` | `claimId`, `actorUid`, `roleId` | `ClaimRole` |
| `renameRole` | `ClaimRoleRenameCommand` | `claimId`, `actorUid`, `roleId`, `name` | `ClaimRole` |
| `updateRolePriority` | `ClaimRolePriorityUpdateCommand` | `claimId`, `actorUid`, `roleId`, `priority` | `ClaimRole` |
| `updateRolePermission` | `ClaimRolePermissionUpdateCommand` | `claimId`, `actorUid`, `roleId`, `permissions` | `ClaimRole` |
| `createWarp` | `ClaimWarpCreateCommand` | `claimId`, `actorUid`, `name`, `isPublic`, `location` | `ClaimWarp` |
| `deleteWarp` | `ClaimWarpDeleteCommand` | `claimId`, `actorUid`, `warpId` | `ClaimWarp` |
| `renameWarp` | `ClaimWarpRenameCommand` | `claimId`, `actorUid`, `warpId`, `name` | `ClaimWarp` |
| `relocateWarp` | `ClaimWarpRelocateCommand` | `claimId`, `actorUid`, `warpId`, `location` | `ClaimWarp` |
| `updateWarpVisibility` | `ClaimWarpVisibilityCommand` | `claimId`, `actorUid`, `warpId`, `isPublic` | `ClaimWarp` |

`blockKeyId` is a key from the `blocks` section of `config.yml`, such as `block1`. `expireClaim`
refuses a claim whose time has not run out (`ERROR_CLAIM_NOT_EXPIRED`); use `deleteClaim` to remove
one early.

## Controlling side effects

Every write has a second overload taking `ClaimCommandOptions`. The one-argument form runs every
side effect, as if a player had typed the command.

```java
claims.createClaim(command, ClaimCommandOptions.builder()
        .without(ClaimSideEffect.ECONOMY)
        .without(ClaimSideEffect.WEBHOOK)
        .build());
```

| Effect | Skipping it means |
|---|---|
| `PERMISSION` | Role permissions and ability nodes are not checked |
| `LIMITATION` | Entitlement limits are not enforced |
| `ECONOMY` | Nothing is charged |
| `COOLDOWN` | Cooldowns are neither checked nor started |
| `REGION` | WorldGuard regions and `disabledWorlds` are not checked |
| `WEBHOOK` | Discord is not notified |

| Factory | Enabled effects |
|---|---|
| `all()`, `forPlayer()` | All six |
| `forAdmin()` | `REGION`, `WEBHOOK` |
| `forSystem()` | `WEBHOOK` |
| `none()` | None |
| `builder()` | All six, minus each `without(…)`; `with(…)` replaces the set |

`ClaimCommandOptions.of(ActorKind)` gives the same presets, and `ActorKind.PLAYER_PREAUTHORIZED` is
every effect except `PERMISSION`. Prefer removing effects one at a time over `none()`, which also
removes the permission check.

## Threading

A write takes a per-claim lock, publishes its events, then saves to the database before it returns.
Call writes off the main thread, the way the plugin's own commands do. Reads are served from memory
and are safe on any thread.

## PlayerFacade

| Method | Returns |
|---|---|
| `findByUid(UUID)` / `findByName(String)` | `Optional<CPlayer>` |
| `getByUid` / `getByName` | `CPlayer`, throwing when absent |
| `getByUidUnsafe` / `getByNameUnsafe` | `CPlayer` or `null` |
| `existsByUid` / `existsByName` | `boolean` |
| `loginPlayer(PlayerLoginCommand)` / `registerPlayer(PlayerRegisterCommand)` | `CPlayer` |

Use `findByName` to turn a name typed in chat into the UUID the command objects need. The plugin
calls the login and register commands itself on join.

Related: [Events](events.md), [Domain model](models.md), [Architecture](architecture.md)
