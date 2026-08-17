---
title: Events
order: 440
description: Every event the plugin fires, and why they may arrive off the main thread.
icon: zap
---

## Registering Event Listeners

<Callout type="warning" title="Async Event Firing">

uxmClaims events may be fired **asynchronously** (off the main thread). If you need to interact with Bukkit APIs that require the main thread (e.g., spawning entities, modifying blocks, sending packets), you **must** schedule your code back to the main thread using `Bukkit.getScheduler().runTask()`. Failure to do so may cause `IllegalStateException` or other threading issues.

</Callout>

### Using @ClaimEventHandler

Create a listener class and register it with the API:

```java
import com.uxplima.claim.bukkit.api.UxmClaimBukkitAPI;
import com.uxplima.claim.bukkit.api.annotation.ClaimEventHandler;
import com.uxplima.claim.domain.event.ClaimCreateEvent;

public class MyClaimListener {
    
    @ClaimEventHandler
    public void onClaimCreate(ClaimCreateEvent event) {
        Claim claim = event.getClaim();
        System.out.println("Claim created: " + claim.getName());
    }
}
```

Register in your plugin:

```java
@Override
public void onEnable() {
    UxmClaimBukkitAPI api = UxmClaimBukkitAPI.getInstance();
    api.registerEvent(new MyClaimListener());
}

@Override
public void onDisable() {
    UxmClaimBukkitAPI api = UxmClaimBukkitAPI.getInstance();
    api.unregisterEvent(myListener);
}
```

---

## @ClaimEventHandler Options

```java
@ClaimEventHandler(priority = 1, ignoreCancelled = false)
public void onEvent(ClaimEvent event) {
    // ...
}
```

| Option            | Default | Description               |
|-------------------|---------|---------------------------|
| `priority`        | `1`     | Lower = called first      |
| `ignoreCancelled` | `false` | Skip if already cancelled |

---

## Cancelling Events

**All events are cancellable.** Use `setCancelled(true)` to prevent the action:

```java
import com.uxplima.claim.domain.event.ClaimCreateEvent;

@ClaimEventHandler
public void onClaimCreate(ClaimCreateEvent event) {
    Claim claim = event.getClaim();
    
    // Prevent claims in specific world
    if (claim.getMainChunk().getWorld().equals("event_world")) {
        event.setCancelled(true);
    }
}
```

---

## All Available Events (All Cancellable ✅)

### Claim Lifecycle

| Event                  | When It Fires           |
|------------------------|-------------------------|
| `ClaimCreateEvent`     | Claim is being created  |
| `ClaimDeleteEvent`     | Claim is being deleted  |
| `ClaimExpireEvent`     | Claim has expired       |
| `ClaimRenameEvent`     | Claim renamed           |
| `ClaimRelocateEvent`   | Spawn location moved    |
| `ClaimRescheduleEvent` | Expiration date changed |

---

### Chunk Events

| Event                   | When It Fires |
|-------------------------|---------------|
| `ClaimChunkExtendEvent` | Chunk added   |
| `ClaimChunkShrinkEvent` | Chunk removed |

---

### Member Events

| Event                              | When It Fires           |
|------------------------------------|-------------------------|
| `ClaimMemberBanEvent`              | Player was banned       |
| `ClaimMemberUnbanEvent`            | Player was unbanned     |
| `ClaimMemberKickEvent`             | Player was kicked       |
| `ClaimMemberLeaveEvent`            | Player left voluntarily |
| `ClaimMemberRoleAssignEvent`       | Role changed            |
| `ClaimMemberPermissionUpdateEvent` | Permissions changed     |

---

### Invite Events

| Event                    | When It Fires        |
|--------------------------|----------------------|
| `ClaimInviteSendEvent`   | Invitation sent      |
| `ClaimInviteAcceptEvent` | Invitation accepted  |
| `ClaimInviteRevokeEvent` | Invitation cancelled |

---

### Role Events

| Event                            | When It Fires       |
|----------------------------------|---------------------|
| `ClaimRoleCreateEvent`           | Role created        |
| `ClaimRoleDeleteEvent`           | Role deleted        |
| `ClaimRoleRenameEvent`           | Role renamed        |
| `ClaimRolePriorityUpdateEvent`   | Priority changed    |
| `ClaimRolePermissionUpdateEvent` | Permissions changed |

---

### Warp Events

| Event                      | When It Fires          |
|----------------------------|------------------------|
| `ClaimWarpCreateEvent`     | Warp created           |
| `ClaimWarpDeleteEvent`     | Warp deleted           |
| `ClaimWarpRenameEvent`     | Warp renamed           |
| `ClaimWarpRelocateEvent`   | Warp location moved    |
| `ClaimWarpVisibilityEvent` | Public/private toggled |

---

### Block Events

| Event                    | When It Fires         |
|--------------------------|-----------------------|
| `ClaimBlockPlaceEvent`   | Claim block placed    |
| `ClaimBlockDestroyEvent` | Claim block destroyed |
| `ClaimBlockChangeEvent`  | Block style changed   |

---

### Other Events

| Event                   | When It Fires          |
|-------------------------|------------------------|
| `ClaimFlagsUpdateEvent` | Flags changed          |
| `ClaimVaultUpdateEvent` | Vault contents changed |
| `ClaimMoveInsideEvent`  | Player entered claim   |
| `ClaimMoveOutsideEvent` | Player left claim      |

---

## Event Data Access

All events extend `ClaimEvent` and implement `CancellableDomainEvent`:

```java
@ClaimEventHandler
public void onAnyEvent(ClaimEvent event) {
    // Get the affected claim
    Claim claim = event.getClaim();
    
    // All events are cancellable
    if (event instanceof CancellableDomainEvent cancellable) {
        boolean isCancelled = cancellable.isCancelled();
        cancellable.setCancelled(true);
    }
}
```

---

## Example: Welcome Message

```java
@ClaimEventHandler
public void onEnterClaim(ClaimMoveInsideEvent event) {
    Claim claim = event.getClaim();
    Player player = event.getPlayer();
    
    player.sendMessage("Welcome to " + claim.getName() + "!");
}
```

---

## Example: Prevent Entry for Banned

```java
@ClaimEventHandler
public void onEnterClaim(ClaimMoveInsideEvent event) {
    Claim claim = event.getClaim();
    UUID playerUid = event.getPlayer().getUniqueId();
    
    if (claim.hasBanByUid(playerUid)) {
        event.setCancelled(true);
    }
}
```

---

## Example: Log All Claim Deletions

```java
@ClaimEventHandler
public void onClaimDelete(ClaimDeleteEvent event) {
    Claim claim = event.getClaim();
    getLogger().info("Claim deleted: " + claim.getName() + 
                     " owned by " + claim.getOwnerUid());
}
```

---

## Best Practices

1. **Check cancellation first** - If already cancelled, skip your logic
2. **Use appropriate priority** - Lower numbers run first
3. **Keep handlers fast** - Don't block the main thread
4. **Unregister on disable** - Clean up when your plugin stops

```java
@ClaimEventHandler(priority = 0, ignoreCancelled = true)
public void onEvent(ClaimCreateEvent event) {
    // priority = 0 runs first
    // ignoreCancelled = true skips if already cancelled
}
```

---

## Next Steps

- [🔧 Use Cases](../developer/usecases.md) - Modifying claims
- [📊 Domain Models](../developer/models.md) - Understanding data structures
