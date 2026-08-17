---
title: Events
order: 1004
description: The event bus, the 35 events, and why they may arrive off the main thread.
icon: zap
---

uxmClaims has its own event bus. These are **not** Bukkit events and are not registered with
`PluginManager`.

## Listening

```java
public class ClaimListener {

    @ClaimEventHandler(priority = 5, ignoreCancelled = true)
    public void onCreate(ClaimCreateEvent event) {
        // …
    }
}

UxmClaimBukkitAPI.getInstance().registerEvent(new ClaimListener());
```

Unregister with `api.unregisterEvent(instance)`.

| Attribute | Default | Meaning |
|---|---|---|
| `priority` | `1` | Lower runs first |
| `ignoreCancelled` | `false` | Skip this handler if an earlier one cancelled the event |

Note that priority runs **lowest first**, which is the opposite of Bukkit's `EventPriority`.

## Cancelling

Events extending `CancellableDomainEvent` can be cancelled, which aborts the operation:

```java
@ClaimEventHandler
public void onCreate(ClaimCreateEvent event) {
    if (isInsideEventArena(event)) {
        event.setCancelled(true);
    }
}
```

## The events

| Area | Events |
|---|---|
| Claim | `ClaimCreateEvent`, `ClaimDeleteEvent`, `ClaimExpireEvent`, `ClaimRenameEvent`, `ClaimRelocateEvent`, `ClaimRescheduleEvent`, `ClaimFlagsUpdateEvent` |
| Chunks | `ClaimChunkExtendEvent`, `ClaimChunkShrinkEvent` |
| Members | `ClaimMemberKickEvent`, `ClaimMemberLeaveEvent`, `ClaimMemberBanEvent`, `ClaimMemberUnbanEvent`, `ClaimMemberRoleAssignEvent`, `ClaimMemberPermissionUpdateEvent`, `ClaimOwnerTransferEvent` |
| Invites | `ClaimInviteSendEvent`, `ClaimInviteAcceptEvent`, `ClaimInviteRevokeEvent` |
| Roles | `ClaimRoleCreateEvent`, `ClaimRoleDeleteEvent`, `ClaimRoleRenameEvent`, `ClaimRolePriorityUpdateEvent`, `ClaimRolePermissionUpdateEvent` |
| Warps | `ClaimWarpCreateEvent`, `ClaimWarpDeleteEvent`, `ClaimWarpRenameEvent`, `ClaimWarpRelocateEvent`, `ClaimWarpVisibilityEvent` |
| Claim block | `ClaimBlockPlaceEvent`, `ClaimBlockChangeEvent`, `ClaimBlockDestroyEvent` |
| Vault | `ClaimVaultUpdateEvent` |
| Movement | `ClaimMoveInsideEvent`, `ClaimMoveOutsideEvent` |

`ClaimMoveInsideEvent` and `ClaimMoveOutsideEvent` fire on border crossings, which on a busy server is
often. Keep those handlers cheap — no database calls, no synchronous lookups.

<Callout type="danger" title="Handlers may run off the main thread">

uxmClaims publishes events from whichever thread performed the operation, and writes are frequently
asynchronous. Touching the Bukkit API — spawning entities, changing blocks, sending packets — from a
handler will throw or corrupt state on a regionised server.

Schedule back to the main thread first:

```java
@ClaimEventHandler
public void onCreate(ClaimCreateEvent event) {
    Bukkit.getScheduler().runTask(plugin, () -> spawnCelebration(event));
}
```

On Folia, use the region scheduler for the claim's location rather than the global one.

</Callout>

## Events versus webhooks

`webhooks.yml` posts a subset of these to Discord without any code. Reach for the event bus when you
need to *react* — cancel the operation, mirror the change into another plugin, write an audit row —
and for a notification, use the webhook.
