---
title: Use Cases
order: 450
icon: workflow
---

## How It Works

1. Build a **Command** object using `.builder()`
2. Get **ClaimFacade** from the API
3. Call the appropriate method on the facade
4. Handle the result

```java
// Example: Rename a claim
ClaimRenameCommand command = ClaimRenameCommand.builder()
    .claimId(claimId)
    .actorUid(player.getUniqueId())
    .name("New Name")
    .build();

ClaimFacade facade = UxmClaimBukkitAPI.getInstance().claimFacade();
Claim updated = facade.renameClaim(command);
```

---

## Getting the Facade

```java
import com.uxplima.claim.bukkit.api.UxmClaimBukkitAPI;
import com.uxplima.claim.app.facade.ClaimFacade;

ClaimFacade claimFacade = UxmClaimBukkitAPI.getInstance().claimFacade();
```

---

## Building Commands

All commands use Lombok's `@Builder` pattern:

```java
import com.uxplima.claim.app.command.ClaimCreateCommand;

ClaimCreateCommand command = ClaimCreateCommand.builder()
    .actorUid(player.getUniqueId())
    .ownerUid(player.getUniqueId())
    .spawnLocation(BukkitConverter.toDomainLocation(player.getLocation()))
    .build();
```

---

## Available Operations

### Claim Lifecycle

| Method              | Command                  | Description        |
|---------------------|--------------------------|--------------------|
| `createClaim()`     | `ClaimCreateCommand`     | Create a new claim |
| `deleteClaim()`     | `ClaimDeleteCommand`     | Delete a claim     |
| `renameClaim()`     | `ClaimRenameCommand`     | Change claim name  |
| `relocateClaim()`   | `ClaimRelocateCommand`   | Move spawn point   |
| `rescheduleClaim()` | `ClaimRescheduleCommand` | Extend expiration  |
| `expireClaim()`     | `ClaimExpireCommand`     | Manually expire    |

---

### Chunks

| Method          | Command                   | Description    |
|-----------------|---------------------------|----------------|
| `extendClaim()` | `ClaimChunkExtendCommand` | Add a chunk    |
| `shrinkClaim()` | `ClaimChunkShrinkCommand` | Remove a chunk |

---

### Members & Invitations

| Method                | Command                              | Description        |
|-----------------------|--------------------------------------|--------------------|
| `inviteMember()`      | `ClaimInviteSendCommand`             | Send invitation    |
| `acceptInvite()`      | `ClaimInviteAcceptCommand`           | Accept invitation  |
| `revokeInvite()`      | `ClaimInviteRevokeCommand`           | Cancel invitation  |
| `banMember()`         | `ClaimMemberBanCommand`              | Ban a player       |
| `unbanMember()`       | `ClaimMemberUnbanCommand`            | Unban a player     |
| `kickMember()`        | `ClaimMemberKickCommand`             | Kick a member      |
| `leaveMember()`       | `ClaimMemberLeaveCommand`            | Leave voluntarily  |
| `assignMemberRole()`  | `ClaimMemberRoleAssignCommand`       | Change role        |
| `updateMemberPerms()` | `ClaimMemberPermissionUpdateCommand` | Update permissions |

---

### Roles

| Method                 | Command                            | Description        |
|------------------------|------------------------------------|--------------------|
| `createRole()`         | `ClaimRoleCreateCommand`           | Create custom role |
| `deleteRole()`         | `ClaimRoleDeleteCommand`           | Delete role        |
| `renameRole()`         | `ClaimRoleRenameCommand`           | Rename role        |
| `updateRolePriority()` | `ClaimRolePriorityUpdateCommand`   | Change priority    |
| `updateRolePerms()`    | `ClaimRolePermissionUpdateCommand` | Update permissions |

---

### Warps

| Method                   | Command                      | Description           |
|--------------------------|------------------------------|-----------------------|
| `createWarp()`           | `ClaimWarpCreateCommand`     | Create warp           |
| `deleteWarp()`           | `ClaimWarpDeleteCommand`     | Delete warp           |
| `renameWarp()`           | `ClaimWarpRenameCommand`     | Rename warp           |
| `relocateWarp()`         | `ClaimWarpRelocateCommand`   | Move warp             |
| `updateWarpVisibility()` | `ClaimWarpVisibilityCommand` | Toggle public/private |

---

### Flags, Vault & Blocks

| Method               | Command                    | Description         |
|----------------------|----------------------------|---------------------|
| `updateClaimFlags()` | `ClaimFlagsUpdateCommand`  | Update flags        |
| `updateVault()`      | `ClaimVaultUpdateCommand`  | Update vault        |
| `placeBlock()`       | `ClaimBlockPlaceCommand`   | Place claim block   |
| `destroyBlock()`     | `ClaimBlockDestroyCommand` | Destroy claim block |
| `changeBlock()`      | `ClaimBlockChangeCommand`  | Move claim block    |

---

## Examples

### Creating a Claim

```java
import com.uxplima.claim.app.command.ClaimCreateCommand;
import com.uxplima.claim.bukkit.api.BukkitConverter;

public void createClaimFor(Player player) {
    ClaimCreateCommand command = ClaimCreateCommand.builder()
        .actorUid(player.getUniqueId())
        .ownerUid(player.getUniqueId())
        .spawnLocation(BukkitConverter.toDomainLocation(player.getLocation()))
        .build();
    
    try {
        Claim claim = claimFacade.createClaim(command);
        player.sendMessage("Claim created: " + claim.getName());
    } catch (ClaimException e) {
        player.sendMessage("Error: " + e.getStatus());
    }
}
```

---

### Banning a Player

```java
import com.uxplima.claim.app.command.ClaimMemberBanCommand;

public void banFromClaim(UUID claimId, Player actor, UUID targetUid) {
    ClaimMemberBanCommand command = ClaimMemberBanCommand.builder()
        .claimId(claimId)
        .actorUid(actor.getUniqueId())
        .bannedUid(targetUid)
        .reason("Banned via API")
        .build();
    
    ClaimBan ban = claimFacade.banMember(command);
}
```

---

### Creating a Warp

```java
import com.uxplima.claim.app.command.ClaimWarpCreateCommand;

public void createWarp(UUID claimId, Player actor, String warpName) {
    ClaimWarpCreateCommand command = ClaimWarpCreateCommand.builder()
        .claimId(claimId)
        .actorUid(actor.getUniqueId())
        .name(warpName)
        .location(BukkitConverter.toDomainLocation(actor.getLocation()))
        .isPublic(false)
        .build();
    
    ClaimWarp warp = claimFacade.createWarp(command);
}
```

---

### Updating Flags

```java
import com.uxplima.claim.app.command.ClaimFlagsUpdateCommand;
import com.uxplima.claim.domain.model.enums.ClaimFlag;

public void disablePvp(UUID claimId, Player actor, Claim claim) {
    // Get current flags and remove PVP
    Set<ClaimFlag> newFlags = new HashSet\<>(claim.getFlags());
    newFlags.remove(ClaimFlag.PVP);
    
    ClaimFlagsUpdateCommand command = ClaimFlagsUpdateCommand.builder()
        .claimId(claimId)
        .actorUid(actor.getUniqueId())
        .flags(newFlags)
        .build();
    
    Claim updated = claimFacade.updateClaimFlags(command);
}
```

---

## Command Options

All methods have an overloaded version with `ClaimCommandOptions`:

```java
import com.uxplima.claim.app.command.options.ClaimCommandOptions;
import com.uxplima.claim.app.command.options.ClaimSideEffect;

// Disable event publishing but keep other side effects
ClaimCommandOptions options = ClaimCommandOptions.builder()
    .without(ClaimSideEffect.EVENT)  // Don't fire events
    .build();

Claim result = claimFacade.deleteClaim(command, options);
```

Use this when:

- You want to suppress event firing
- You're doing batch operations
- You need custom side-effect control

---

## Error Handling

All operations may throw `ClaimException`:

```java
import com.uxplima.claim.domain.exception.ClaimException;

try {
    claimFacade.banMember(command);
} catch (ClaimException e) {
    switch (e.getStatus()) {
        case ERROR_CLAIM_NOT_FOUND -> player.sendMessage("Claim not found!");
        case ERROR_NO_PERMISSION -> player.sendMessage("No permission!");
        case ERROR_OWNER_CANNOT_BE_BANNED -> player.sendMessage("Cannot ban owner!");
        default -> player.sendMessage("Error: " + e.getMessage());
    }
}
```

---

## Next Steps

- [📊 Domain Models](../developer/models.md) - Understanding data structures
- [⚡ Events](../developer/events.md) - Listening to changes
