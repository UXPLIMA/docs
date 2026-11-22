---
title: Legacy API
order: 186
description: Moving a plugin from the 3.x API to 4.0, and what the 3.x bridge still does until 5.0.
icon: history
---

The 3.x API (`com.uxplima.claim.*`: `UxmClaimBukkitAPI`, `ClaimFacade`, `@ClaimEventHandler` and the
domain model) still exists in 4.0. The [Legacy API](../modules/legacy-api.md) module implements it on top of
the new API, so a plugin built against `com.uxplima.claim:bukkit-api:3.x` keeps running without a rebuild.
The bridge is frozen and every class in it is `@Deprecated(forRemoval = true)`.

<Callout type="warning" title="Removed in 5.0">

The bridge and the deprecated API are removed in uxmClaims 5.0, which comes no earlier than 12 months
after 4.0.0. Move your plugin to `com.uxplima.uxmclaims:uxmclaims-api` before then.

</Callout>

## Finding plugins that still use it

`/uxm platform deprecations` (node `uxmclaims.admin.legacy-api`, op by default) lists every plugin that has
called the old API since the server started, and which parts of it. With `log-callers: true` in `modules/legacy-api/config.yml`,
each plugin is also logged once when it first calls it. The coordinates `com.uxplima.claim:bukkit-api` are
published with each 4.x version and contain the 3.x classes only, so a 3.x plugin also still compiles.

## What the bridge does

| Area | In 4.0 |
|---|---|
| `UxmClaimBukkitAPI.getInstance()` | Works while the `legacy-api` module runs |
| `getInstance(Class)` | Resolves only `ClaimFacade`, `PlayerFacade`, `EventBusPort`, `BorderVisualizer`, `InputMenu`, `PluginReloader` and `ClaimPermissionPolicy`; anything else throws `UnsupportedOperationException` |
| `ClaimFacade` reads | Work for every land, from memory. Each `Claim` is built from the current `LandView` |
| `ClaimFacade` writes | Work on personal lands through the 4.x services. Town, nation and server lands are read-only: a write throws `ClaimException(ERROR_UNSUPPORTED_IN_V4)` |
| Writes whose module is off | `ERROR_UNSUPPORTED_IN_V4`, for example `createWarp` with the warps module off |
| Refusals | A 4.x `ActionException` becomes a `ClaimException`; the status is taken from the message key (`ERROR_INVALID_INPUT` when none fits) and `getCause()` is the `ActionException` |
| Domain mutators | `Claim.rename(...)`, `Claim.relocate(...)` and the other setters on `Claim` and `CPlayer` throw `UnsupportedOperationException`; use the facade |
| `PlayerFacade` | Reads come from the 4.x profiles. `loginPlayer` and `registerPlayer` do nothing and return the player |
| `ClaimPermissionPolicy` | For an online player inside the claim, the answer is the full 4.x protection chain; otherwise the 3.x rules on the claim copy |
| `BorderVisualizer` | Shows the borders of the land the player stands in, in the configured colours (borders module) |
| `InputMenu` | Asks through the 4.x text input (Bedrock form, Java dialog or chat) |
| `PluginReloader` | Runs the same reload as `/uxm reload` |
| `@ClaimEventHandler` events | Built from the 4.x events; see the event table below |

## Behaviour changes

| 3.x | 4.0 bridge |
|---|---|
| A write called from any thread ran on that thread | Called on the main thread, the write runs right there and returns the committed result. Called from another thread, it is handed to the global thread and your thread waits at most `write-timeout` seconds (default 10), then gets `ERROR_TIMEOUT`; the write may still finish later |
| Event handlers ran on the thread of the write, often async | Handlers run on the server thread (the owning region thread on Folia). One slower than `slow-handler-millis` (default 50) is logged |
| A handler that threw aborted the write | The error is logged with your plugin's name and the next handler runs |
| Cancelling any event aborted the write | Only events made from a 4.x `Pre*` event can cancel. Events published after the commit (see the table) ignore cancellation and log it once |
| `ClaimSideEffect.REGION` and `WEBHOOK` could be turned off | Region checks and webhooks always run; turning them off is logged and ignored |
| `updateRolePriority` set a priority | The role is moved with repeated one-step moves |
| `BorderVisualizer` took colours and chunk sets | Both are ignored (logged once) |
| `InputMenu.close()` withdrew the question | It cannot withdraw a question already asked |

## Entry point and reads

| 3.x | 4.0 |
|---|---|
| `UxmClaimBukkitAPI.getInstance()` | `UxmClaimsApi.get()` or `UxmClaimsApi.find()` |
| `api.claimFacade()` | `api.lands()` plus the module services, `api.service(WarpService.class)` and so on |
| `api.playerFacade()` | `api.profiles()` |
| `api.getInstance(Class)` | `api.service(Class)` |
| `BukkitConverter.toDomainLocation(Location)` | Not needed: `lands().at(Location)`; `new WorldRef(world.getUID(), world.getName())` and `ChunkPos.ofBlock(x, z)` for the rest |
| `findById(UUID)` / `getById` / `getByIdUnsafe` | `lands().byId(UUID)` (an `Optional`) |
| `findByChunk(Chunk)` | `lands().at(WorldRef, ChunkPos)` |
| `findByLocation(Location)` | `lands().at(org.bukkit.Location)` or `protection().landAt(Location)` |
| `existsBy...` | `...isPresent()` |
| `existsByWarpName(String)` | `service(WarpService.class)` then `byName(String).isPresent()` |
| `findByOwnerUid(UUID)` / `countByOwnerUid` | `lands().ownedBy(OwnerRef.player(uuid))` / `.size()` |
| `findByMemberUid(UUID)` / `countByMemberUid` | `lands().memberOf(uuid)`, which includes owned lands |
| `findByInvitedUid(UUID)` | `service(InviteService.class)` then `invitesTo(uuid)` |
| `findAllClaims()` | `lands().all()` |
| `findExpiredClaims()` | `service(ExpiryService.class)` then `info(land).map(i -> i.expiredAt(Instant.now()))` per land |
| `findPublicWarps()` | `service(WarpService.class)` then `publicWarps()` |
| `PlayerFacade.findByUid` / `findByName` | `profiles().byId(UUID)` / `profiles().byName(String)` |
| `ClaimPermissionPolicy`, `Claim.hasPermission(uuid, perm)` | `protection().can(...)`, `canManage(UUID, LandView, PermissionKey)` |
| `BorderVisualizer` | `service(BorderService.class)` then `show(player)` / `hide(uuid)` |
| `InputMenu` | No API counterpart for a plain plugin; a module uses `menus().input()` (see [Menus](menus.md)) |
| `PluginReloader` | No API counterpart; dispatch `/uxm reload` from the console |
| `ClaimFlag`, `ClaimPermission` enums | `FlagKey`, `PermissionKey`. `registries().flags()` and `rolePermissions()` give each key's `legacyName()`, the 3.x enum name |

## Writes

Every 3.x write took a command object and returned a result; every 4.0 write returns a `CompletableFuture`
and takes `WriteOptions` (see [The API](api.md)).

| 3.x `ClaimFacade` | 4.0 |
|---|---|
| `createClaim` | `lands().create(CreateLand, options)` |
| `deleteClaim` / `expireClaim` | `lands().delete(id, DeleteReason.OWNER or EXPIRED, options)` |
| `renameClaim` | `lands().rename` |
| `relocateClaim` | `lands().setSpawn` |
| `rescheduleClaim` | `ExpiryService.extend(land, Duration, options)` or `set` |
| `updateClaimFlags` | `lands().setFlag(land, FlagKey, Tri, options)`, one per flag |
| `extendClaim` / `shrinkClaim` | `lands().claimChunk` / `unclaimChunk` |
| `inviteMember` / `revokeInvite` / `acceptInvite` | `InviteService.send` / `revoke` / `accept` |
| `kickMember` / `leaveMember` | `lands().removeMember` |
| `banMember` / `unbanMember` | `BanService.ban` / `unban` |
| `assignMemberRole` | `lands().setMemberRole` |
| `updateMemberPerms` | `lands().setMemberOverride`, one per key |
| `transferOwnership` | `lands().transferOwner(land, OwnerRef, options)` |
| `createRole` / `deleteRole` / `renameRole` | `lands().createRole` / `deleteRole` / `renameRole` |
| `updateRolePriority` | `lands().moveRole(land, role, up, options)` |
| `updateRolePermission` | `lands().setRolePermission` |
| `createWarp` / `deleteWarp` / `renameWarp` / `relocateWarp` / `updateWarpVisibility` | `WarpService.create` / `delete` / `rename` / `move` / `setPublic` |
| `placeBlock` / `changeBlock` / `destroyBlock` | `ClaimBlockService.place` / `change` / `destroy` |
| `updateVault` | `VaultService.replace` |
| `ClaimCommandOptions.forPlayer()` / `forAdmin()` / `forSystem()` | `WriteOptions.asPlayer(id, name)` / `asAdmin(actor)` / `asSystem(reason)` |
| `ClaimSideEffect.PERMISSION`, `LIMITATION`, `ECONOMY`, `COOLDOWN` | `Check.PERMISSION`, `LIMIT`, `COST`, `COOLDOWN` |
| `ClaimException` and `getStatus()` | `ActionException` and `messageKey()`, as the future's failure |

## Events

| 3.x event | 4.0 event | Cancel works |
|---|---|---|
| `ClaimCreateEvent` | `PreLandCreateEvent` | Yes |
| `ClaimDeleteEvent` | `PreLandDeleteEvent` (not for expiry) | Yes |
| `ClaimExpireEvent` | `PreLandExpireEvent` | Yes |
| `ClaimRenameEvent` | `PreLandRenameEvent` | Yes |
| `ClaimRelocateEvent` | `LandSpawnChangeEvent` | No |
| `ClaimRescheduleEvent` | `LandRescheduleEvent` | No |
| `ClaimFlagsUpdateEvent` | `PreLandFlagChangeEvent` | Yes |
| `ClaimChunkExtendEvent` / `ClaimChunkShrinkEvent` | `PreLandChunkClaimEvent` / `PreLandChunkUnclaimEvent` | Yes |
| `ClaimInviteSendEvent` | `PreLandInviteSendEvent` | Yes |
| `ClaimInviteAcceptEvent` | `PreLandMemberAddEvent` | Yes |
| `ClaimInviteRevokeEvent` | `LandInviteRemoveEvent` | No |
| `ClaimMemberKickEvent` / `ClaimMemberLeaveEvent` | `PreLandMemberRemoveEvent` | Yes |
| `ClaimMemberBanEvent` / `ClaimMemberUnbanEvent` | `PreLandBanEvent` / `LandUnbanEvent` | Yes / No |
| `ClaimMemberRoleAssignEvent` | `LandMemberRoleChangeEvent` | No |
| `ClaimMemberPermissionUpdateEvent` | `LandMemberOverrideChangeEvent` | No |
| `ClaimOwnerTransferEvent` | `PreLandOwnerTransferEvent` (to a player, on personal lands) | Yes |
| `ClaimRoleCreateEvent`, `ClaimRoleDeleteEvent`, `ClaimRoleRenameEvent` | `LandRoleCreateEvent`, `LandRoleDeleteEvent`, `LandRoleRenameEvent` | No |
| `ClaimRolePriorityUpdateEvent` / `ClaimRolePermissionUpdateEvent` | `LandRolePriorityChangeEvent` / `LandRolePermissionChangeEvent` | No |
| `ClaimWarpCreateEvent` | `PreWarpCreateEvent` | Yes |
| `ClaimWarpDeleteEvent`, `ClaimWarpRenameEvent`, `ClaimWarpRelocateEvent`, `ClaimWarpVisibilityEvent` | `WarpDeleteEvent`, `WarpRenameEvent`, `WarpRelocateEvent`, `WarpVisibilityEvent` | No |
| `ClaimBlockPlaceEvent`, `ClaimBlockChangeEvent`, `ClaimBlockDestroyEvent` | The `claimblock` events of the same names | No |
| `ClaimVaultUpdateEvent` | `VaultUpdateEvent` | No |
| `ClaimMoveInsideEvent` / `ClaimMoveOutsideEvent` | `LandEnterEvent` / `LandLeaveEvent` | No |

The 3.x handlers run at `LOW` on the 4.x bus, so a 4.x handler at `NORMAL` or later already sees their
cancellation. A module that is off fires nothing, so its old events stay silent too. In new code, replace
`@ClaimEventHandler` and `registerEvent` with `api.events().subscribe(...)` or a Bukkit listener on the
mirror (see [Events](events.md)); a handler of a supertype such as `LandEvent` now gets every subtype.

## Before and after

```java
// 3.x
Claim claim = UxmClaimBukkitAPI.getInstance().claimFacade()
        .findByLocation(BukkitConverter.toDomainLocation(player.getLocation()))
        .orElse(null);
```

```java
// 4.0
import com.uxplima.uxmclaims.api.UxmClaimsApi;
import com.uxplima.uxmclaims.api.land.LandView;

import java.util.Optional;

Optional<LandView> land = UxmClaimsApi.get().lands().at(player.getLocation());
```

Related: [The API](api.md), [Events](events.md), [Legacy API module](../modules/legacy-api.md), [Upgrading from 3.x](../upgrading/from-3x.md)
