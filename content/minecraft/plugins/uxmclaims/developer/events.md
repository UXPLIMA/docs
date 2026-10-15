---
title: Events
order: 1004
description: The event bus, the 35 events and their getters, and which thread they arrive on.
icon: zap
---

uxmClaims has its own event bus. Its events are not Bukkit events: you register with
`UxmClaimBukkitAPI`, not with `PluginManager`, and annotate with `@ClaimEventHandler`, not
`@EventHandler`.

## Listening

```java
import com.uxplima.claim.bukkit.api.UxmClaimBukkitAPI;
import com.uxplima.claim.bukkit.api.annotation.ClaimEventHandler;
import com.uxplima.claim.domain.event.ClaimCreateEvent;
import java.util.logging.Logger;

public class ClaimListener {

    private final Logger logger;

    public ClaimListener(Logger logger) {
        this.logger = logger;
    }

    @ClaimEventHandler(priority = 5, ignoreCancelled = true)
    public void onCreate(ClaimCreateEvent event) {
        this.logger.info("New claim: " + event.getClaim().getName());
    }
}

// in onEnable
UxmClaimBukkitAPI.getInstance().registerEvent(new ClaimListener(getLogger()));
```

Unregister with `api.unregisterEvent(listener)`, for example in your `onDisable`.

| Attribute | Default | Meaning |
|---|---|---|
| `priority` | `1` | Lower runs first, the opposite of Bukkit's `EventPriority` |
| `ignoreCancelled` | `false` | Skip this handler when an earlier one cancelled the event |

| Rule | Consequence |
|---|---|
| A handler returns `void` and takes exactly one event parameter | Any other method is skipped silently |
| Handlers are matched by the exact event class | A handler taking `ClaimEvent` never runs; name the concrete class |
| Only methods declared on the listener's own class are scanned | Handlers inherited from a superclass are not registered |
| Two handlers with the same class, method name and priority count as one | Registering a second instance of the same listener class adds nothing |
| An exception thrown by a handler propagates | The operation fails and later handlers do not run |

## Cancelling

Every event implements `CancellableDomainEvent` (`isCancelled()`, `setCancelled(boolean)`).

```java
@ClaimEventHandler
public void onCreate(ClaimCreateEvent event) {
    if (event.getClaim().getSpawnLocation().getWorld().equals("event_arena")) {
        event.setCancelled(true);
    }
}
```

A write publishes its events after its permission, limit, cooldown and balance checks, and before
anything is charged or saved. `getClaim()` already shows the change. Cancelling aborts the write:
the facade throws `ClaimException` with status `EVENT_CANCELLED`, and nothing is charged or saved.

Cancelling `ClaimMoveInsideEvent` or `ClaimMoveOutsideEvent` pushes a walking player back over the
border and cancels a teleport. A teleport by a player with `uxmclaims.bypass.teleport`, or through a
portal, goes ahead regardless.

## The events

All events live in `com.uxplima.claim.domain.event` and extend `ClaimEvent`, whose `getClaim()`
returns the claim. The table lists the getters each adds.

| Event | Getters | Fired when |
|---|---|---|
| `ClaimCreateEvent` | none | A claim is created |
| `ClaimDeleteEvent` | none | A claim is deleted, also after `ClaimExpireEvent` |
| `ClaimExpireEvent` | none | An expired claim is removed |
| `ClaimRenameEvent` | `getOldName()`, `getNewName()`, `setNewName(String)` | A claim is renamed |
| `ClaimRelocateEvent` | `getNewLocation()` | The claim spawn moves |
| `ClaimRescheduleEvent` | `getExpireDate()` | Time is added or removed |
| `ClaimFlagsUpdateEvent` | `getFlags()` | The flag set changes |
| `ClaimChunkExtendEvent` | `getChunk()` | A chunk is added |
| `ClaimChunkShrinkEvent` | `getClaimChunk()` | A chunk is removed |
| `ClaimInviteSendEvent` | `getInvite()` | An invite is sent |
| `ClaimInviteAcceptEvent` | `getInvite()`, `getMember()` | An invite is accepted |
| `ClaimInviteRevokeEvent` | `getInvite()` | An invite is revoked |
| `ClaimMemberKickEvent` | `getMember()` | A member is kicked |
| `ClaimMemberLeaveEvent` | `getMember()` | A member leaves |
| `ClaimMemberBanEvent` | `getBan()` | A player is banned |
| `ClaimMemberUnbanEvent` | `getBan()` | A ban is lifted |
| `ClaimMemberRoleAssignEvent` | `getMember()`, `getRole()` | A member gets a role |
| `ClaimMemberPermissionUpdateEvent` | `getMember()`, `getAllowedPermissions()`, `getDeniedPermissions()` | A member's overrides change |
| `ClaimOwnerTransferEvent` | `getOldOwner()`, `getNewOwner()` | Ownership moves |
| `ClaimRoleCreateEvent` | `getRole()` | A role is created |
| `ClaimRoleDeleteEvent` | `getRole()` | A role is deleted |
| `ClaimRoleRenameEvent` | `getRole()`, `getName()` | A role is renamed |
| `ClaimRolePriorityUpdateEvent` | `getRole()`, `getPriority()` | A role's priority changes |
| `ClaimRolePermissionUpdateEvent` | `getRole()`, `getPermissions()` | A role's permissions change |
| `ClaimWarpCreateEvent` | `getWarp()` | A warp is created |
| `ClaimWarpDeleteEvent` | `getWarp()` | A warp is deleted |
| `ClaimWarpRenameEvent` | `getWarp()` | A warp is renamed |
| `ClaimWarpRelocateEvent` | `getWarp()` | A warp moves |
| `ClaimWarpVisibilityEvent` | `getWarp()` | A warp turns public or private |
| `ClaimBlockPlaceEvent` | `getClaimBlock()` | The claim block is placed |
| `ClaimBlockChangeEvent` | `getClaimBlock()` | The claim block style changes |
| `ClaimBlockDestroyEvent` | `getClaimBlock()` | The claim block is removed |
| `ClaimVaultUpdateEvent` | `getVault()` | The vault contents are saved |
| `ClaimMoveInsideEvent` | `getActorUid()`, `getActorName()`, `getChunk()` | A player enters a claim |
| `ClaimMoveOutsideEvent` | `getActorUid()`, `getActorName()`, `getChunk()` | A player leaves a claim |

`ClaimRenameEvent.setNewName` lets a handler rewrite the name. The new name passes the same
validation as a typed one, and an invalid one aborts the rename.

The two movement events fire on every border crossing, on the thread that moved the player. Keep
those handlers cheap: no database calls, no blocking lookups.

<Callout type="danger" title="Write events usually arrive off the main thread">

The plugin runs its writes asynchronously and publishes their events on that thread, and a write
you call through the facade publishes on your thread. Touching the Bukkit world from such a handler
(spawning entities, changing blocks) throws or corrupts state. Schedule back first:

```java
@ClaimEventHandler
public void onCreate(ClaimCreateEvent event) {
    Bukkit.getScheduler().runTask(plugin, () -> spawnCelebration(event.getClaim()));
}
```

On Folia, use the region scheduler for the claim's location instead of the global one.

</Callout>

## Events and webhooks

`webhooks.yml` posts many of these to Discord without code. Use the event bus to react: cancel an
operation, mirror a change into another plugin, write an audit row. Use a webhook to notify.

Related: [The API](api.md), [Domain model](models.md), [webhooks.yml](../config/webhooks-yml.md)
